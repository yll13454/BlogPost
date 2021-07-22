# Webpack配置详解

webpack.base.config.js为开发和生产环境通用的配置，

webpack.dev.config.js和webpack.prod.config.js分别是开发和生产环境的单独配置。

```text
entry: {
    main: './src/main',
    vendors: ['vue', 'vue-router']
}
```

entry是入口的配置项，这里我们使用数组来定义了两个入口，main是我们的主入口，主要是引入vue-router路由和app.vue入口组件。其中app.vue提供了路由的挂载元素，以及通用的组件

```js
output: {
    path: path.join(__dirname, './dist'),
    publicPath: '/dist/',
    filename: '[name].js',
    chunkFilename: '[name].[hash].js'
}
```

output是输出配置，在本项目中将其分发到了不同的环境里。

path是文件输出到本地的路径，

publicPath是文件的引用路径，比如开发环境可将其配置为cdn路径，

filename是主入口的文件名，

chunkFilename是每个路由编译后的文件名，其中[hash]是用来唯一标识文件，主要用来防止缓存。

#### loaders

```js
module: {
    loaders: [
        { test: /\.vue$/, loader: 'vue' },
        { test: /\.js$/, loader: 'babel', exclude: /node_modules/ },
        { test: /\.css$/, loader: 'style!css!autoprefixer'},
        { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
        { test: /\.(gif|jpg|png|woff|svg|eot|ttf)\??.*$/, loader: 'url-loader?limit=8192'},
        { test: /\.(html|tpl)$/, loader: 'html-loader' }
    ]
}
```

url-loader，它会将小于8kb的图片、iconfont字体都转为base64，超过8kb的才会生成具体文件

resolve.extensions是对模块后缀名的简写，配置后，原本是require('./components/app.vue') 可以简写为require('./components/app')。

resolve.alias是别名，配置后，比如原本是require('moment/min/moment-with-locales.min.js') 可以简写为require('moment')。

#### plugins

```js
new ExtractTextPlugin("[name].css",{ allChunks : true,resolve : ['modules'] })  // 开发
new ExtractTextPlugin("[name].[hash].css",{ allChunks : true,resolve : ['modules'] })  // 生产
```

这两个插件就是用来提取css的，目前只能提取使用@import形式和.vue里面style内的css。

```js
new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.js')  // 开发
new webpack.optimize.CommonsChunkPlugin('vendors', 'vendors.[hash].js')  // 生产
```

这两个插件是提取js公共组件的，比如vue,vue-router,jquery,echarts等，我们可以在入口的vendors里进行配置。

```js
/ 开发
new HtmlWebpackPlugin({
    filename: '../index.html',
    template: './src/template/index.html',
    inject: 'body'
})
// 生产
new HtmlWebpackPlugin({
    filename: '../index_prod.html',
    template: './src/template/index.html',
    inject: 'body'
})
```

这两个插件依赖html-webpack-plugin，是用来生产html的，

其中filename是生产的文件路径和名称，template是使用的模板，inject是指将js放在body还是head里。

生成的index.html是开发环境用的，index_prod.html是生产环境用的，生产环境里引用的css和js都是使用hash命名的，而且进行了压缩。

```js
// 生产
new webpack.optimize.UglifyJsPlugin({
    compress: {
        warnings: false
    }
})
```

这个插件在生产时使用，是将代码进行压缩。