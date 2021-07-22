# 基于vue-cli3创建libs库的实现方法

[链接地址](https://www.jb51.net/article/175712.htm)

**用途：创建公共函数和组件**

**一：创建默认项目**

使用 `vue create xxx` 创建一个默认项目

**二：修改原始目录结构**

创建的默认目录如下，我们需要将原本的src目录改为examples，然后创建packages目录来编写自己的组件库文件，

**三：配置example & packages**

到这里你可能发现了，项目无法跑起来了，因为我们没有了原始默认的入口文件

在根目录下创建vue.config.js配置文件（vue-cli3的配置文件，在这里的配置会覆盖脚手架webpack默认的配置项）。

```js
//不熟悉配置的可官网查看 https://cli.vuejs.org/zh/config/#pages
module.exports = {
 ``pages: {
  ``index: {
   ``entry: ``'examples/main.js'``,
   ``template: ``'public/index.html'``,
   ``filename: ``'index.html'
  ``}
  ``}
}
```

进入packages目录创建一个组件目录（HelloWorld），一个index文件（用来暴露所有的组件），然后在组件目录新建index（暴露出当前组件）文件与src组件源码。

![img](media\2019120411005637.jpg) 

在HelloWorld下面的index中引入组件。

```js
import HelloWord from './src/HelloWorld.vue';
HelloWord.install = (Vue)=>{
 Vue.components(HelloWord.name,HelloWord)
}
export default HelloWord;
```

在packages目录下面的index中引入所有的组件并注册。

```js
import HelloWorld from './HelloWorld';

// 将引入的组件模块存储，方便循环注册所有组件
const components = [ HelloWorld ];

const install = (Vue,options)=>{
  if (install.installed) return;
  install.installed = true
  console.log(options)
  components.forEach(component => {
    Vue.component(component.name, component)
  })
}

// 如果是直接引入的
if (typeof window !== 'undefined' && window.Vue) {
  install(window.Vue)
}

export default {
  // 使用Vue.use必须具有install方法
  // https://cn.vuejs.org/v2/api/#Vue-use
  install,
  // 同时导出组件列表
  ...components
}
```

在examples中的入口文件main.js引入注册的全局组件。

```js
//main.js
...
import libs from "../packages"
...
Vue.use(libs,{a:'test'})
```

到现在项目基本已经跑起来了，在项目中可以直接调用组件了，无需在components里注册，平时开发中你的全局组件也可以这样来注册使用。

![img](D:\笔记\media\2019120411005638.jpg) 

**四：组件打包**

vue-cli3中提供了[构建一个库的方法](https://cli.vuejs.org/zh/guide/build-targets.html#库) ，默认是构建应用，这个方法提供的几个配置项主要有以下几个

target: 改为 lib 可启用构建库模式；
name: 构建库输出的文件名；
dest: 构建的输出目录，默认为 dist；
entry: 入口文件路径；

```js
vue-cli-service build --target lib --name myLib --dest libs [entry]
```

所以需要在项目的package.json文件中添加一条上面的执行脚本

```
"scripts": {
  "lib": "vue-cli-service build --target lib --name common --dest lib packages/index.js"
}
```

vue-cli3的默认配置是只对src目录下的文件进行编译的，所有这里我们还需要在vue.config.js文件中将packages文件夹也加入loader编译的配置（后测试可不加）。

```js
chainWebpack:(config)=>{
  config.module
    .rule('js')
    .include
      .add('/packages/')
      .end()
    .use('babel')
      .loader('babel-loader')
      .tap(options=>{
          //TODO
          return options
      })
}
```

然后执行npm run lib会在跟目录下生成lib文件夹，lib会生成三种形式的文件（commonjs，umd，umd-min）

![img](D:\笔记\media\2019120411005639.jpg) 