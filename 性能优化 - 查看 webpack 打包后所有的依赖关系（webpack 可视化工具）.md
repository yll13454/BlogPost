# [性能优化 - 查看 webpack 打包后所有的依赖关系（webpack 可视化工具）](https://blog.csdn.net/qq_16559905/article/details/78551719)

#### **1：`webpack-bundle-analyzer（可视化）`==**

**安装和使用**

```
npm install --save-dev webpack-bundle-analyzer
```

在`webpack.config.js`中：

```
let BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
    plugins: [new BundleAnalyzerPlugin()]
}
```

==BundleAnalyzerPlugin== 构造函数可以采用默认的可选配置对象：

```
new BundleAnalyzerPlugin({
  //  可以是`server`，`static`或`disabled`。
  //  在`server`模式下，分析器将启动HTTP服务器来显示软件包报告。
  //  在“静态”模式下，会生成带有报告的单个HTML文件。
  //  在`disabled`模式下，你可以使用这个插件来将`generateStatsFile`设置为`true`来生成Webpack Stats JSON文件。
  analyzerMode: 'server',
  //  将在“服务器”模式下使用的主机启动HTTP服务器。
  analyzerHost: '127.0.0.1',
  //  将在“服务器”模式下使用的端口启动HTTP服务器。
  analyzerPort: 8888, 
  //  路径捆绑，将在`static`模式下生成的报告文件。
  //  相对于捆绑输出目录。
  reportFilename: 'report.html',
  //  模块大小默认显示在报告中。
  //  应该是`stat`，`parsed`或者`gzip`中的一个。
  //  有关更多信息，请参见“定义”一节。
  defaultSizes: 'parsed',
  //  在默认浏览器中自动打开报告
  openAnalyzer: true,
  //  如果为true，则Webpack Stats JSON文件将在bundle输出目录中生成
  generateStatsFile: false, 
  //  如果`generateStatsFile`为`true`，将会生成Webpack Stats JSON文件的名字。
  //  相对于捆绑输出目录。
  statsFilename: 'stats.json',
  //  stats.toJson（）方法的选项。
  //  例如，您可以使用`source：false`选项排除统计文件中模块的来源。
  //  在这里查看更多选项：https：  //github.com/webpack/webpack/blob/webpack-1/lib/Stats.js#L21
  statsOptions: null,
  logLevel: 'info' 日志级别。可以是'信息'，'警告'，'错误'或'沉默'。
})
```

**启动服务：**

生产环境查看：`npm run build --report` 或 正常build 即可启动查看器

开发环境查看：`webpack -p --progress` 或启动正常devServer服务即可启动查看器!







## 性能优化

1. 页面中使用了大量图片和音频文件的，可以使用CND加速，托管七牛，腾讯云等。
2. 图片压缩，降低图片分辨率，高分辨比文件大小更影响页面加载速度。
3. 框架文件可以使用Application Cache 应用缓存。
4. 动画页面可以使用link标签rel="prefetch"预加载文件。
5. 压缩文件。
6. 大量的请求会导致加载过慢，使用雪碧图或合拼请求等。
