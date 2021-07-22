## 1.路径匹配问题：

```css
@import url('~/assets/css/style.css') //success
```

## 2.按需引入(UI框架等等)

例如使用UI框架：`element-ui`

> 第一步，下载依赖：

```
# 先下载element-ui
npm install element-ui --save
# 如果使用按需引入，必须安装babel-plugin-component(官网有需要下载说明，此插件根据官网规则不同，安装插件不同)
npm install babel-plugin-component --save-dev
```

安装好以后，按照`nuxt.js`中的规则，你需要在 `plugins/` 目录下创建相应的插件文件

在**文件根目录**创建(或已经存在)`plugins/`目录，创建名为：`element-ui.js`的文件，内容如下：

```js
import Vue from 'vue'
import { Button } from 'element-ui'    //引入Button按钮
export default ()=>{
    Vue.use(Button)
}
```

> 第二步，引入插件

在`nuxt.config.js`中，添加配置为：`plugins`

```
 css:[
'element-ui/lib/theme-chalk/index.css'
],
plugins:[
'~/plugins/element-ui'
]
```

默认为：开启SSR,采用服务端渲染，也可以**手动配置关闭SSR**，配置为：

```
 css:[
'element-ui/lib/theme-chalk/index.css'
],
plugins:[
    {
        src:'~/plugins/element-ui',
        ssr:false    //关闭ssr
    }
]
```

> 第三步，配置 `babel`选项

在`nuxt.config.js`中，配置在`build`选项中，规则为官网规则：

```js
 
build: {
      babel:{        //配置按需引入规则
          "plugins":[
              [
                  "component",
                  {
                      "libraryName":"element-ui",
                      "styleLibraryName":"theme-chalk"
                  }
              ]
          ]
      },
    /*
     ** Run ESLINT on save
     */
    extend (config, ctx) {
      if (ctx.isClient) {
        config.module.rules.push({
           enforce: 'pre',
           test: /\.(js|vue)$/,
           loader: 'eslint-loader',
           exclude: /(node_modules)/
        })
      }
    }
 }
```

## 3.初始化脚手架的选择：

官网提供的初始化脚手架为：

```
# 基本的Nuxt.js项目模板



 



vue init nuxt/starter template
```

而其实，官方也提供了更多的模板以便于我们使用，而我在**中文文档**并未发现有说明：

- `nuxt/starter` 基本的Nuxt.js项目模板
- `nuxt/express` Nuxt.js + Express
- `nuxt/koa` Nuxt.js + Koa2
- `nuxt/adonuxt` Nuxt.js + AdonisJS
- `nuxt/micro` Nuxt.js + Micro
- `nuxt/nuxtent` 适用于内容较重网站的Nuxt.js + Nuxtent模块

------

而我们使用基础的模板进行初始化项目，部署方式为：

> 第一步，打包：

在执行`npm run build`的时候，`nuxt`会自动打包

> 第二步，选择要部署的文件：

- `.nuxt`文件夹
- `package.json` 文件
- `nuxt.config.js` 文件(如果你部署一些proxy，则需要上传这个文件，个人建议把它传上去)

> 第三步，启动你的 `nuxt` **（重要）**

使用`pm2`启动你的`nuxt.js`

```
pm2 start npm --name "demo" -- run start
```

**在这里，我发现个问题，如果你使用window server 服务器，在使用`pm2`启动时候，会出现错误，错误如下：**

![windows server](media/1668224023-5adc47462ea06_articlex)

#### 如果在`Linux服务器`下启动，同样的命令，同样的执行，则不会出现错误：

#### 这里采用`Linux CentOS 7`

## ![CentOS 7服务器](media/2840577851-5adc47ba7502f_articlex)

#### 所以，个人建议，在采用**初始化模板的时候，请选用`express` 或者 `koa` 进行初始化，理由如下：**

##### 1.采用基础模板初始化，观察`package.json`的启动方式如下：

```
"scripts": {



    "dev": "nuxt",



    "build": "nuxt build",



    "start": "nuxt start",



    "generate": "nuxt generate",



    "lint": "eslint --ext .js,.vue --ignore-path .gitignore .",



    "precommit": "npm run lint"



  }
```

##### 2.采用`express`/`koa`初始化模板，观察`package.json`的启动方式如下：

```
"scripts": {



    "dev": "backpack dev",



    "build": "nuxt build && backpack build",



    "start": "cross-env NODE_ENV=production node build/main.js",



    "precommit": "npm run lint",



    "lint": "eslint --ext .js,.vue --ignore-path .gitignore ."



  }
```

##### 在`start`中，对比下，个人觉得`express`/`koa`更灵活一些，它直接启动了`build/main.js`文件，更能直观的启动方式，而关键在于，也可以在`windows server`下运行起来。

> 注意事项：如果采用 `express`/ `koa`的模板初始化，服务器部署的时候，同时要上传 `build/`目录！！！

------

## 4.插件中获取vue绑定

我们需要在axios的插件中配置Loading加载效果，例如使用`element-ui`框架作为示例：

> 1.创建插件

在文件根目录创建(或已经存在)`plugins/`目录，创建名为：`axios.js`的文件，内容如下：

```
import Vue from 'vue'



 



var vm = new Vue({})    //获取vue实例



 



export default function ({ $axios, redirect }) {



 



  $axios.onRequest(config => {



    if (process.browser) {    //判断是否为客户端（必须）



        vm.$loading();



    }



  })



 



  $axios.onResponse(response=>{



      if (process.browser) {    //判断是否为客户端（必须）



          let load = vm.$loading();



          load.close();



      }



  })



 



  $axios.onError(error => {



    const code = parseInt(error.response && error.response.status)



    if (code === 400) {



      redirect('/400')



    }



  })



}



 
```

如官方所说，并不需要像**原生`axios`**一样，去`return`一个`config`出来。

> 2.配置 `nuxt.config.js`文件

在`plugins`选项添加：

```
 plugins:['~/plugins/axios']
```

添加`modules`选项并添加如下示例：

```
modules:['@nuxtjs/axios']
```

配置防止多次打包：

在build选项中(`nuxt.config.js`会默认配置)添加`vendor`配置项：

```
build:{



    vendor:['axios']



}
```

这样就可以调用loading加载方法,并且愉快的使用了。

（当然还有其他的方法去调用vue实例，每个人习惯不同，使用方式不同。）

------

## 5.Nuxt.js中配置代理解决跨域

我们知道在vue-cli中配置代理很方便，只需要在`config/`目录下的`index.js`中找到`proxyTable`添加即可，而在nuxt中同样需要修改`nuxt.config.js`配置文件。

> 1.原始配置代理方式

使用`@nuxtjs/axios`和`@nuxtjs/proxy`进行代理解决跨域

##### 1）.下载插件

```
# 下载插件



 



npm install @nuxtjs/axios @nuxtjs/proxy --save



 
```

##### 2）.配置插件

在`nuxt.config.js`添加配置项：`modules`和`proxy`。

```
export default = {



 



    modules:[



        '@nuxtjs/axios',



        '@nuxtjs/proxy'



    ],



    proxy:[



        ['/json.html',{target:'http://www.xxxx.com'}]    //注意这也是一个数组



    ]



    



}



 
```

按照上面的方式已经完成了代理，可以进行跨域请求了。

> 2.第二种方式的代理配置

##### 1）.下载插件

这次只需要下载`@nuxtjs/axios`插件就可以。

```
# 下载插件



 



npm install @nuxtjs/axios --save
```

##### 2）.配置插件

```
module.exports = {



 



  modules: [



    '@nuxtjs/axios',



  ],



  axios: {



    proxy:true



  },



  proxy:{



    '/api': 'http://api.example.com',



    '/api2': 'http://api.another-website.com'



  }



 



}
```

#### 特别注意：此时，`axios`选项为对象(`object`)，`proxy`选项为对象(`object`)。

------

### `@nuxtjs/axios`的配置项

------

#### `pathRewrite`选项(重写地址)

如果配置`pathRewrite`选项，可以采用第二种写法如下：

```
proxy: {



 



  '/api/': { target: 'http://api.example.com', pathRewrite: {'^/api/': ''} }



 



 }



 
```

`/api/`将被添加到API端点的所有请求中。可以使用`pathRewrite`选项删除。

因为在 ajax 的 url 中加了前缀 `/api`，而原本的接口是没有这个前缀的。

所以需要通过 pathRewrite 来重写地址，将前缀 `/api` 转为 `/`或者是`''`。

如果本身的接口地址就有 `/api` 这种通用前缀，就可以把 `pathRewrite` 删掉。

------

#### `retry`选项(自动拦截失败请求)

可以在`axios`选项中，配置`retry`配置项，自动拦截失败请求，**默认为3次。**

```
axios: {



  retry: { retries: 3 }



}
```

------

#### `progress`选项(发出请求时显示加载栏)

与`Nuxt.js`进度条集成，在发出请求时显示加载栏。**（仅在浏览器上，当加载栏可用时。）**

您还可以使用`progress`配置为每个请求禁用进度条。

```
this.$axios.$get('URL', { progress: false })
```

------

#### `baseURL`选项（服务器端默认请求地址）

在服务器端使用和预先创建请求的基本URL。

------

#### `browserBaseURL`选项（客户端默认请求地址）

在客户端使用和预先创建请求的基本URL。