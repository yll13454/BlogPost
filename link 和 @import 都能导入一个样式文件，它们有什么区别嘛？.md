link 和 @import 都能导入一个样式文件，它们有什么区别嘛？

- link 是 HTML 标签，除了能导入 CSS 外，还能导入别的资源，比如图片、脚本和字体等；而 @import 是 CSS 的语法，只能用来导入 CSS；
- link 导入的样式会在页面加载时同时加载，@import 导入的样式需等页面加载完成后再加载；
- link 没有兼容性问题，@import 不兼容 ie5 以下；
- link 可以通过 JS 操作 DOM 动态引入样式表改变样式，而@import不可以。


作者：大海我来了
链接：https://juejin.cn/post/6941206439624966152
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。