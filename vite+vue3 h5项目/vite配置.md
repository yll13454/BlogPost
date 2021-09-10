`Vite` 官网地址：https://cn.vitejs.dev/

## 1. 环境变量配置

项目根目录新建，`.env.production`

- .env.development

```javascript
NODE_ENV=development
  
VITE_APP_WEB_URL=YOUR WEB URL
123
```

- .env.production

```javascript
NODE_ENV=production
  
VITE_APP_WEB_URL=YOUR WEB URL
123
```

在页面中使用

```typescript
console.log(import.meta.env.VITE_APP_WEB_URL)
1
```

在 `package.json`中使用

```json
"scripts":{
  "build:dev": "vite build --mode development",
  "build:pro": "vite build --mode production"
}
```

## 2. PWA 配置

安装依赖 `vite-plugin-pwa`

```shell
yarn add --dev vite-plugin-pwa
1
```

配置 `vite.config.ts` 文件

```typescript
import { VitePWA } from 'vite-plugin-pwa'

plugins:[
  ...
  VitePWA({
    manifest: {},
    workbox: {
      skipWaiting: true,
      clientsClaim: true
    }
  })
]
```

具体配置参考：https://github.com/antfu/vite-plugin-pwa

## 3. 组件样式按需加载配置

安装依赖 `vite-plugin-style-import`

```shell
yarn add --dev vite-plugin-style-import
```

以 `ant-design-vue`为例，`配置`vite.config.ts` 文件

```typescript
import styleImport from 'vite-plugin-style-import'

css:{
  preprocessorOptions:{
    less:{
      modifyVars:{},
      javascriptEnabled: true
    }
  }
},
plugins:[
  styleImport({
    libs:[
      {
        libraryName: 'ant-design-vue',
        esModule: true,
        resolveStyle: name => `ant-design-vue/es/${name}/style/index`
      }
    ]
  })
]  
```

具体配置参考：https://github.com/anncwb/vite-plugin-style-import

## 4. webpack 代理配置

配置 `vite.config.ts` 文件

```typescript
server: {
    host: '0.0.0.0',
    port: 3000,
    open: true,
    https: false,
    proxy: {}
},
```

## 5. 生产环境移除 console

配置 `vite.config.ts` 文件

```typescript
build:{
  ...
  terserOptions: {
      compress: {
        drop_console: true,
        drop_debugger: true
      }
  }
}
```

## 6. 生产环境生成 .gz / .br文件

安装依赖

```shell
yarn add --dev vite-plugin-compression
```

配置 `vite.config.ts` 文件

```typescript
import viteCompression from 'vite-plugin-compression'

plugins:[
  ...
  // gzip压缩
  new CompressionWebpackPlugin({
    filename: '[path][base].gz',
    algorithm: 'gzip',
    test: new RegExp(
      '\\.(' +
      ['js', 'css'].join('|') +
      ')$'
    ),
    threshold: 10240,
    minRatio: 0.8
  }),
  // brotli压缩
  new CompressionWebpackPlugin({
    filename: '[path][base].br',
    algorithm: 'brotliCompress',
    test: /\.(js|css|html|svg)$/,
    threshold: 10240,
    minRatio: 0.8
 })
]
```

### [Vite 2.0 配置总结](https://blog.csdn.net/qq_35844177/article/details/114852991)

