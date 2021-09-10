# [vue项目打包优化指南](https://segmentfault.com/a/1190000023009421)

# 按需加载路由

# 异步组件

```javascript
export default {
  name: 'MyPage',
  components: {
    test: () => import('./test') // 异步引入组件，让webpack将代码分割打包
  },
}
```

# 分析工具

vue-cli中内置了 `webpack-bundle-analyzer` 插件，默认情况下，我们修改 `package.json` 中的打包命令为 `npm run build --report` 即可查看可视化打包分析：

```json
{
  "scripts": {
    "build": "vue-cli-service build --report",
  },
}
```

# 按需打包Moment.js

`moment.js` 占用空间大的原因在于，moment中包含了大量语言资源文件，我们并不需要这些。

通过webpack自身的功能即可在打包时丢弃这些无用的内容：

```javascript
// 在项目根目录新建 vue.config.js
const webpack = require('webpack')
module.exports = {
  configureWebpack: config => {
    const plugins = [
      // 只保留中文语言资源
      new webpack.ContextReplacementPlugin(/moment[/\\]locale$/, /zh-cn/),
    ]
  },
}
```

## lodash-webpack-plugin插件

也可以使用 `lodash-webpack-plugin` 插件，自动移除没有用到的lodash代码：

> 注意：这个插件在使用 `find` 等方法时可能会删除一些必要的依赖，导致程序报错，解决方法是自己补全依赖，或者换用 `lodash-es`

```javascript
// vue.config.js
const LodashWebpackPlugin = require('lodash-webpack-plugin')

module.exports = {
  configureWebpack: config => {
    const plugins = [
      // 加入这条
      new LodashWebpackPlugin(),
    ]
  },
}
```

# 压缩代码

新版本的vue-cli是自动压缩代码的，所以不需要额外的配置。如果使用旧版cli生成项目，可以查询 [terser](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Fterser%2Fterser) 相关的用法。

> `uglifyify.js` 已经停止维护了，不要再使用这个库

# 生成gzip文件

nginx可以帮助我们将资源自动压缩为gzip文件，如果我们在打包时能够提供一份压缩好的gzip文件的话，传输会更快。

这里使用 `compression-webpack-plugin` 插件：

```bash
$ npm i -D compression-webpack-plugin
```

编辑webpack配置：

```javascript
// vue.config.js
const CompressionWebpackPlugin = require('compression-webpack-plugin')
const productionGzipExtensions = ['js', 'css']

module.exports = {
  configureWebpack: config => {
    const plugins = [
      new CompressionWebpackPlugin({
        filename: '[path].gz[query]',
        algorithm: 'gzip',
        test: new RegExp('\\.(' + productionGzipExtensions.join('|') + ')$'),
        threshold: 10240,
        minRatio: 0.8
      }),
    ]
    // 仅在生产环境压缩，提高开发时的编译速度
    if (process.env.NODE_ENV === 'production') {
      config.plugins = [...plugins, ...config.plugins]
      config.optimization = {
        splitChunks: {
          cacheGroups: {
            // 提取公共模块
            commons: {
              chunks: 'all',
              minChunks: 2,
              maxInitialRequests: 5,
              minSize: 0,
              name: 'common'
            }
          }
        },
      }
    }
  },
}
```

> 注意：gzip还需要服务端支持，一般在nginx上做配置