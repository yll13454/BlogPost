## sass

```text
npm install --save-dev node-sass sass-loader
```

```js
module.exports = {
  css: [
    // 直接加载一个 Node.js 模块。（在这里它是一个 Sass 文件）
    'bulma',
    // 项目里要用的 CSS 文件
    '@/assets/css/main.css',
    // 项目里要使用的 SCSS 文件
    '@/assets/css/main.scss'
  ]
}
```

## normalize.css

对于npm

```
npm install --save normalize.css
```

纱线用

```
yarn add normalize.css
```

然后将以下设置添加到nuxt.config.js。

nuxt.config.js

```
module.exports = {
  //　省略
  css: [
    'normalize.css'
  ]
  //　省略
}
```

1. 服务端渲染的过程为：解析执行JS => 构建HTML页面 => 输出给浏览器
2. 预渲染：直接输出HTML页面给浏览器

## cross-env

npm install --save-dev cross-env



然后在package.json中的scripts命令如下如下：

"scripts": {

 "dev": "cross-env NODE_ENV=development webpack-dev-server --progress --colors --devtool cheap-module-eval-source-map --hot --inline",

 "build": "cross-env NODE_ENV=production webpack --progress --colors --devtool cheap-module-source-map",

}



## js-cookie

[js-cookie](https://github.com/js-cookie/js-cookie)

## node相关   nodemon入门介绍

[nodemon入门介绍](https://zhuanlan.zhihu.com/p/96720675)

