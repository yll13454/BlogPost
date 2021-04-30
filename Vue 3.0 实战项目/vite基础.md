# vite运行原理

使用了**ES Module**,代码以模块的形式引入到文件，同时实现了按需加载。

其最大的特点是在浏览器端使用 `export import` 的方式导入和导出模块，在 script 标签里设置 `type="module"` ，然后使用 **ES module**。

正因如此，vite高度依赖`module script`特性，也就意味着从这里开始抛弃了IE市场，参见[Javascript MDN](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FGuide%2FModules)。

**vite如何处理模块**

vite 对 `import` 都做了一层处理，其过程如下：

1. 在 koa 中间件里获取请求 body
2. 通过 [es-module-lexer](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fes-module-lexer) 解析资源 ast 拿到 import 的内容
3. 判断 import 的资源是否是绝对路径，绝对视为 npm 模块
4. 返回处理后的资源路径，例如：`"vue" => "/@modules/vue"`

将处理的template,script,style等所需的依赖以http请求的形式，通过query参数形式区分并加载SFC文件各个模块内容。

#### 为什么这里需要`@modules`?

举个栗子：

```js
import vue from 'vue'
```

vue模块安装在`node_modules`中，浏览器`ES Module`是无法直接获取到项目下node_modules目录中的文件。所以`vite`对`import`都做了一层处理，重写了前缀使其带有`@modules`，以便项目访问引用资源；另一方面，把文件路径都写进同一个@modules中，类似面向切片编程，可以从中再进行其他操作而不影响其他部分资源，比如后续可加入alias等其他配置。

通过koa middleware正则匹配上带有`@modules`的资源，再通过require('XXX')获取到导出资源并返给浏览器。

### 文件请求

单页面文件的请求有个特点，都是以`*.vue`作为请求路径结尾，当服务器接收到这种特点的http请求，主要处理

- 根据`ctx.path`确定请求具体的vue文件
- 使用`parseSFC`解析该文件，获得`descriptor`，一个`descriptor`包含了这个组件的基本信息，包括`template`、`script`和`styles`等属性 下面是`Comp.vue`文件经过处理后获得的`descriptor`

然后根据`descriptor`和`ctx.query.type`选择对应类型的方法，处理后返回`ctx.body`

- type为空时表示处理`script`标签，使用`compileSFCMain`方法返回`js`内容
- type为`template`时表示处理`template`标签，使用`compileSFCTemplate`方法返回`render`方法
- type为`style`s时表示处理`style`标签，使用`compileSFCStyle`方法返回`css`文件内容

在浏览器里使用 ES module 是使用 http 请求拿到的模块，所以 vite 必须提供一个`web server` 去代理这些模块，上文中提到的 `koa中间件` 就是负责这个事情，vite 通过对请求路径`query.type`的劫持获取资源的内容返回给浏览器，然后通过拼接不同的处理单页面文件解析后的各个资源文件，最后响应给浏览器进行渲染。

从另一方面来看，这也是一个非常有趣的方法，webpack之类的打包工具会把各种各样的模块提前打包进bundle中，但打包结果是静态的，不管某个模块的代码是否用得到，它都要被打包进去，显而易见的坏处就是随着项目越来越大，打包文件也越来越大。vite的优雅之处就在于需要某个模块时动态引入，而不是提前打包，自然而然提高了开发体验。

## hmr热更新

vite的热更新主要有四步：

1. 通过 watcher 监听文件改动；
2. 通过 server 端编译资源，并推送新资源信息给 client ；
3. 需要框架支持组件 rerender/reload ；
4. client 收到资源信息，执行框架 rerender 逻辑。

在client端，Websocket监听了一些更新的消息类型，然后分别处理：

- **vue-reload** —— vue 组件更新：通过 import 导入新的 vue 组件，然后执行 `HMRRuntime.reload`
- **vue-rerender** —— vue template 更新：通过 import 导入新的 template ，然后执行 `HMRRuntime.rerender`
- **vue-style-update** —— vue style 更新：直接插入新的 stylesheet
- **style-update** —— css 更新：document 插入新的 stylesheet
- **style-remove** —— css 移除：document 删除 stylesheet
- **js-update** —— js 更新：直接执行
- **full-reload** —— 页面 roload：使用 `window.reload` 刷新页面

在server端，通过watcher监听页面改动，根据文件类型判断是js Reload还是vue Reload。通过解析器拿到当前文件内容，并与缓存里的上一次解析结果进行比较，如果发生改变则执行相应的render。



作者：前端优选
链接：https://www.jianshu.com/p/07960e4bbb01
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。