# 如何写好.babelrc？Babel的presets和plugins配置解析

Babel6.x版本之后，所有的插件都是可插拔的。这也意味着你安装了Babel之后，是不能工作的，需要配置对应的.babelrc文件才能发挥完整的作用。下面就对Babel的presets和plugins配置做一个简单解析。

## 预设(presets)

使用的时候需要安装对应的插件，对应babel-preset-xxx，例如下面的配置，需要`npm install babel-preset-es2015`。

```json
{  "presets": ["es2015"]}
```

### env

```json
{  "presets": ["env", options]}
```

最近新增的一个选项，有以下options选择。

#### targets: { [string]: number }，默认{}

需要支持的环境，可选例如：chrome, edge, firefox, safari, ie, ios, node，甚至可以制定版本，如node: 6.5。也使用node: current代表使用当前的版本。

#### browsers: Array\ | string，默认[]

浏览器列表，使用的是[browserslist](https://github.com/ai/browserslist)，可选例如：last 2 versions, > 5%。

#### loose: boolean，默认false

是否使用宽松模式，如果设置为true，plugins里的插件如果允许，都会采用宽松模式。

#### debug: boolean，默认false

编译是否会去掉console.log。

#### whitelist: Array\，默认[]

设置一直引入的plugins列表。

### es2015/es2016/es2017/latest

```json
{  "presets": ["es2015"]}
```

#### es2015

使用es2015的，也就是我们常说的es6的相关方法，简单翻译如下，更多细节可以参看[文档](https://babeljs.io/docs/plugins/preset-es2015/)。

## 插件(plugins)

其实看了上面的应该也明白了，presets，也就是一堆plugins的预设，起到方便的作用。如果你不采用presets，完全可以单独引入某个功能，比如以下的设置就会引入编译箭头函数的功能。

```json
{  "plugins": ["transform-es2015-arrow-functions"]}
```

### transform-runtime

```json
{  "plugins": ["transform-runtime", options]}
```

主要有以下options选择。

#### helpers: boolean，默认true

使用babel的helper函数。

#### polyfill: boolean，默认true

使用babel的polyfill，但是不能完全取代babel-polyfill。

#### regenerator: boolean，默认true

使用babel的regenerator。

#### moduleName: string，默认babel-runtime

使用对应module处理。

这里的options一般不用自己设置，用默认的即可。这个插件最大的作用主要有几下几点：

- 解决编译中产生的重复的工具函数，减小代码体积
- 非实例方法的poly-fill，如`Object.assign`，但是实例方法不支持，如`"foobar".includes("foo")`，这时候需要单独引入babel-polyfill

更多细节参见[文档](https://babeljs.io/docs/plugins/transform-runtime/)。

### transform-remove-console

```json
{  "plugins": ["transform-remove-console"]}
```

使用这个插件，编译后的代码都会移除`console.*`，妈妈再也不用担心线上代码有多余的console.log了。当然很多时候，我们如果使用webpack，会在webpack中配置。

## plugins/presets排序

也许你会问，或者你没注意到，我帮你问了，plugins和presets编译，也许会有相同的功能，或者有联系的功能，按照怎么的顺序进行编译？答案是会按照一定的顺序。

- 具体而言，plugins优先于presets进行编译。
- plugins按照数组的index增序(从数组第一个到最后一个)进行编译。
- presets按照数组的index倒序(从数组最后一个到第一个)进行编译。因为作者认为大部分会把presets写成`["es2015", "stage-0"]`。具体细节可以看[这个](https://github.com/babel/notes/blob/master/2016-08/august-01.md#potential-api-changes-for-traversal)。

# babel.config.js 和 .babelrc 对比

Babel 有两种并行的配置文件格式，可以一起使用，也可以分开使用。

1. 项目范围的配置

   babel.config.js 文件，具有不同的拓展名（json、js、html）
   babel.config.js 是按照 commonjs 导出对象，可以写js的逻辑。

2. 相对文件的配置

   .babelrc 文件，具有不同的拓展名

总结：baberc 的加载规则是按目录加载的，是只针对自己的代码。config的配置针对了第三方的组件和自己的代码内容。babel.config.js 是一个项目级别的配置，一般有了babel.config.js 就不会在去执行.babelrc的设置。

### babel.config.js 和 .babelrc，优先执行.babelrc

**当按需加载组件的时候，**babel-plugin-component 。**如果两个文件同时配置，并没有生效。原因就是配置无法合并，**

**导致文件无效。解决办法：把配置放入同一个文件，删除另一个，只保留.babelrc就可以了。**