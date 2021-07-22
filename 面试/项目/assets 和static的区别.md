assets里面的会被webpack打包进你的代码里，而static里面的，就直接引用了。
一般在static里面放一些类库的文件，在assets里面放属于该项目的资源文件。
可以看一下我翻译的vue-cli weboack模板的[中文文档](https://athena0304.gitbooks.io/vue-template-webpack-cn/content/static.html)





相同点：资源在html中使用，都是可以的。

不同点：使用assets下面的资源，在js中使用的话，路径要经过webpack中file-loader编译，路径不能直接写。

assets中的文件会经过webpack打包，重新编译，推荐该方式。而static中的文件，不会经过编译。项目在经过打包后，会生成dist文件夹，static中的文件只是复制一遍而已。简单来说，static中建议放一些外部第三方，自己的放到assets，别人的放到static中。

**注意**：如果把图片放在assets与static中，html页面可以使用；但在动态绑定中，assets路径的图片会加载失败，因为webpack使用的是commenJS规范，必须使用require才可以。

[assets与static的区别](https://zhuanlan.zhihu.com/p/143950140)

