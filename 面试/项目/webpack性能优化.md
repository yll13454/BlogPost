`split-chunks`进行公共模块优化

```as
optimization: {
        splitChunks: {
            chunks: "all",
            cacheGroups: {
                vendorsa: {
                    chunks: 'all',
                    test: /(mobx-state-tree|react-color|react-dom-router|sortablejs|mobx-react)/,
                    priority: 100,
                    name: 'vendors-react-mobx',
                },
                venodrb: {
                    test: /lodash/,
                    priority: 100,
                    name: 'vendor-lodash',
                    chunks: 'all'
                },
            }
        }
    },
```

`external`避免将比较大的第三方依赖打包到bundle中

```as
webpack.config.js:

    externals: [
        {
            'react': 'React',
            'react-dom': 'ReactDOM',
            'moment': 'moment',
            'mobx': 'mobx',
            'monaco-editor': 'monaco',
            'echarts': 'echarts',
            'jquery': 'jQuery',
            'hls.js': 'hls',
            'flv.js': 'flv',
        },
        function (context, request, callback) {
            if (/^moment\/.+$/.test(request)) {
                return callback(null, 'root ' + 'moment');
            }
            if (/^tinymce\/.+$/.test(request)){
                return callback(null, 'root ' + 'tinymce');
            }
            if (/^froala-editor\/.+$/.test(request)){
                return callback(null, 'root ' + 'froala');
            }
            if (/^echarts\/.+$/.test(request)){
                return callback(null, 'root ' + 'echarts');
            }
            // 继续下一步且不外部化引用
            callback();
        },
    ]

    index.html:
    <script src="https://unpkg.com/react@16.8.6/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@16.8.6/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/moment@2.29.1/min/moment.min.js"></script>
    <script src="https://unpkg.com/moment@2.29.1/min/locales.min.js"></script>
    <script src="https://unpkg.com/mobx@4.5.2/lib/mobx.umd.min.js"></script>
 
```

对`ts-loader`的优化

```as
{
        test: /\.tsx?$/,
        use: [
          {
            loader: 'ts-loader',
            options: {
              transpileOnly: true
            }
          }
        ],
        exclude: /node_modules/
    },
      ...

    plugins: [
        new ForkTsCheckerWebpackPlugin(),
    ]
 
```

**优化手段**
两个测量工具

- `speed-measure-webpack-plugin`(**SMP**)，要对webpack的构建速度进行优化，得知道要优化的重点在哪里，而该插件则是帮助检查哪些地方需要进一步优化的工具。
- `webpack-bundle-analyzer`，虽然webpack也有官方的[分析工具](https://link.zhihu.com/?target=https%3A//github.com/webpack/analyse)，社区也有许多其他的工具可以参考，但是通过资料、技术分享以及项目经验，`webpack-bundle-analyzer`用的还是不错的，它可以将bundle展示为交互式、可缩放的树状图形式，使用起来非常便捷。

三个可优化阶段
webpack构建大概可分为**loader解析** -> **依赖搜索** -> **打包**等三个阶段，就这三个阶段我们分别展开阐述如何去优化。
**loader解析：**

- `include/exclude`，对于loader而言，不需要对项目中所有的文件进行文件转换，应将loader应用于最少数量的必要模块，常见的配置：
  { ..., exclude: /node_modules/ }, 复制代码
- 如果项目中用到了`ts-loader`，那就要小心了，因为如果不做额外的配置，会发现构建速度是非常耗时的。原因是因为`ts-loader`在每次构建的时候都会对所有文件进行类型检查，当项目越来越庞大，会发现构建速度越来越慢。这个时候需要设置`transpileOnly: true`来提高构建速度，该配置只处理编译而不做类型检查；然而类型检查是使用`TypeScript`的初衷，可以使用`fork-ts-checker-webpack-plugin`插件来在**单独的进程**中做类型检查。那性能和类型检查都能cover到。
- 如果使用的是`babel-loader`，可以设置其cache相关的选项，比如`cacheDirectory`、`cacheCompression`等；
- `cache-loader`: 利用文件的modifier time来检查文件是否更新，如果没有更新则利用缓存的内容，是一个轻量级的比较。特点是在第一次构建的时候比较慢，后面的构建会快很多。但是用它也要特别注意，最好用在性能消耗比较昂贵的地方，否则基本没有什么效果。该loader已经被作者**Archive**了，因为webpack5内置了cache的相关配置，将来如果升级就不需要它了。使用cache-loader时要放在其他loader的前面。
- `thread-loader`：也是应用于比较昂贵的地方，可以将打包任务划分多个node进程，把模块一次分配给这些进程，实现多进程构建。跟`cache-loader`一样也需要放到其他loader的前面。
  { ..., use: [ 'thread-loader', // 昂贵的loader (e.g babel-loader) ], }, 复制代码
- 最后一点是，尽量少用工具，因为每个loader/plugin都有其启动时间，不用就不会有性能问题啦。

**依赖搜索：**

- 减少 `resolve.modules`, `resolve.extensions`等 中条目数量，因为IO操作比较消耗性能；
- 基本上webpack对这些配置有默认值，比如`resolve.modules`为`node_modules`，告诉webpack解析模块时应该搜索`node_modules`目录；`resolve.extensions`默认值为`['.wasm', '.mjs', '.js', '.json']`，如果用了`TypeScript`还是要配置一下的，`extensions`是说在引入模块时可以不需要带扩展:
  import File from '../path/to/file'; 复制代码

**打包: Smaller = Faster**

- `splitChunks`，本着“小即是快”的原则，尽量使chunk包越小越好。在webpack4之前，可以使用`CommonsChunkPlugin`来避免模块与模块之间的重复依赖，webpack4内置了`optimization.splitChunks`，可开箱即用。`splitChunks`有它默认的行为，不同的项目根据需求做不同的调整。可以设置不同的cacheGroup，拆分前必须共享模块的最小chunk数量等等，能最大程度地优化重复依赖的问题。一个简单的 ：
  splitChunks: { chunks: "all", cacheGroups: { lodash: { test: /lodash/, priority: 1, }, } } 复制代码
- `externals`，防止将某些 import 的包打包到 bundle 中，而是在运行时再去从外部获取这些扩展依赖。例如：
  externals: [ { 'moment': 'moment', } ... ] 复制代码

当然需要在`index.html`里面引入cdn依赖，否则在runtime无法找到相应的模块：`<script src="https://unpkg.com/moment@2.29.1/min/moment.min.js"></script>`。
**多环境**
**生产环境：** 生产环境关注与压缩bundle、更轻量的source map等，建议不同环境写不同的配置，当然可以有共用的配置，利用`webpack-merge`可以实现配置共用；对于devTools，推荐使用`source-Map`，相对于`inline-source-map`和`eval-cheap-module-source-map`性能好一点；代码压缩，在WP5中内置了`terser-webpack-plugin`，现在使用WP4的话，需要安装插件，这个插件功能非常强大，除了基本的压缩功能以外，还可以使用多进程并发构建，以及去除注释等功能；不带路径的配置，`path-info`会在bundle中包含模块信息的注释，但在庞大的项目中，会导致GC性能很差，应该关闭；
**开发环境：** 同样地，生产环境有些配置也不适用于开发环境，比如`TerserPlugin`就不需要，因为在开发环境中压缩代码是没有意义的；devTools的最佳实践是`eval-cheap-module-source-map`，我现在的项目比较轻量，但是也能看出对比：
`inline-source-map`：5205ms VS `eval-cheap-module-source-map`： 4744ms 复制代码
虽然是不到1000ms的差距，苍蝇肉也是肉不是？而且将来代码量越来越庞大的时候，差距就更明显了。
当然还有其他的可以优化的方法，比如使用ES module，能更好地利用webpack的tree shaking功能；Dll，为更改不频繁的代码生成单独的编译结果，但却是一个配置比较复杂的过程；还有对图片的压缩等等。以上是对于webpack4性能优化基本的配置，期待webpack5成熟稳定的那一天。