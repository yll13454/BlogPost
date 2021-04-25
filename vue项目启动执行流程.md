## [Vue项目启动代码执行流程分析](https://my.oschina.net/u/4410805/blog/3391364)

### VUE启动流程

#### 1. package.json

执行npm run dev的时，寻找 package.json 文件，包含项目的名称版本、项目依赖等相关信息。

```json
{　# 版本信息
  "name": "kitty-ui",
  "version": "1.0.0",
  "description": "kitty ui project",
  "author": "Louis",
  "private": true,
  "scripts": {
    "dev": "webpack-dev-server --inline --progress --config build/webpack.dev.conf.js",
    "start": "npm run dev",
    "build": "node build/build.js"
  },
  "dependencies": { # 项目依赖
    "vue": "^2.5.2",
    "vue-router": "^3.0.1"
  },
  "devDependencies": {  # 项目依赖
    "autoprefixer": "^7.1.2",
    "babel-core": "^6.22.1","vue-template-compiler": "^2.5.2",
    "webpack": "^3.6.0",
    "webpack-bundle-analyzer": "^2.9.0",
    "webpack-dev-server": "^2.9.1",
    "webpack-merge": "^4.1.0"
  },
  "engines": {　　# node、npm版本要求
    "node": ">= 6.0.0",
    "npm": ">= 3.0.0"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ]
}
```

### 2. webpack.dev.conf.js

![img](D:\笔记\media\616891-20180817144307679-525629868.png) 

加载 build/webpack.dev.conf.js 配置并启动 webpack-dev-server 。

### 3. config/*.js

 webpack.dev.conf.js 中引入了很多模块的内容，其中就包括 config 目录下服务器环境的配置文件。

![img](D:\笔记\media\616891-20180817144812448-1629632495.png) 



### 4. config/index.js

可以看到，在 index.js 文件中包含服务器 host 和 port 以及入口文件的相关配置，默认启动端口是8080，这里可以进行修改。

 ![img](D:\笔记\media\ff202cf3d88f16821e2cb9f7bad8646bb72.png)