## 问题

最近用vue 开发H5的项目，使用了history模式进行了伪静态处理。这里发现了一个问题是安卓和IOS表现不一样的。操作步骤如下：
1.例如我们的首页是 [http://xxx.com](https://link.zhihu.com/?target=http%3A//xxx.com)，路由跳转到test页面，url应该是变成 [http://xxx.com/test](https://link.zhihu.com/?target=http%3A//xxx.com/test)
\2. IOS和安卓的页面显示都一直，但是通过微信复制链接，安卓复制出来是 [http://xxx.com/test](https://link.zhihu.com/?target=http%3A//xxx.com/test)，可是IOS复制出来是 [http://xxx.com](https://link.zhihu.com/?target=http%3A//xxx.com)
\3. 如果只是普通项目展示没有啥问题，但是一旦涉及到JS-SDK上的功能开发，就两者完全不一样了，IOS只需在首页验签一次就可以在一直用，安卓则需要在其他路由页面重新验签才可以。

## 解决

在IOS分享出错，但是刷新一下即可分享成功的主要原因是：vue-router切换的时候操作的都是浏览器的历史记录，iOS会把第一次刚进入时的URL作为真实URL。安卓会把当前URL作为真实URL。

根据IOS和安卓的区别我们可以用设备检测来区分用他们

if (/(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent)){

// 处理iOS

} else if(/(Android)/i.test(navigator.userAgent)) {

// 处理安卓

}

在IOS下我们可以把第一次进入的路由保存起来，然后在使用微信SDK获取签名的时候使用

而安卓分享只需要每次调用encodeURIComponent(location.href.split('#')[0])即可；



作者：SUGAR
链接：https://www.zhihu.com/question/59388458/answer/340351305
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。