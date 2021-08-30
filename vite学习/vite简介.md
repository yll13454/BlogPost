`Vite`的生产环境下打包是通过`Rollup`来完成的

>`Rollup`是JavaScriptd的模块bundler(打包器)，可以将一小段代码编译为更大或更复杂的内容，例如库或应用程序。

也就是说`Vite`提供的是开发环境中的编译，打包工作是依靠的`Rollup`。

### [Vite特性介绍]

1. Vite主打特点就是轻快冷服务启动。冷服务的意思是，在开发预览中，它是不进行打包的。
2. 开发中可以实现热更新，也就是说在你开发的时候，只要一保存，结果就会更新。
3. 按需进行更新编译，不会刷新全部DOM节点。这功能会加快我们的开发流程度。

```js
yarn create vite-app <project-name>
cd <project-name>
yarn
yarn dev
```

Vite生成的目录结构（项目结构）作一个了解。本文很短，重在理解。

```js
|-node_modules      -- 项目依赖包的目录
|-public            -- 项目公用文件
  |--favicon.ico    -- 网站地址栏前面的小图标
|-src               -- 源文件目录，程序员主要工作的地方
  |-assets          -- 静态文件目录，图片图标，比如网站logo
  |-components      -- Vue3.x的自定义组件目录
  |--App.vue        -- 项目的根组件，单页应用都需要的
  |--index.css      -- 一般项目的通用CSS样式写在这里，main.js引入
  |--main.js        -- 项目入口文件，SPA单页应用都需要入口文件
|--.gitignore       -- git的管理配置文件，设置那些目录或文件不管理
|-- index.html      -- 项目的默认首页，Vue的组件需要挂载到这个文件上
|-- package-lock.json --项目包的锁定文件，用于防止包版本不一样导致的错误
|-- package.json    -- 项目配置文件，包管理、项目名称、版本和命令
```

`vite.config.js`中写入下面的别名配置。

```js
export default {
    alias: {
        '@': './src'
    }
}
```

