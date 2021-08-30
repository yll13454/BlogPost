## 上传图片报错：处理异常

这个报错甚是诡异，由于前端和后端代码均没有“处理异常”这4个字。原本想甩锅给微信无论了的，但随后在作限制上传图片大小功能的时候阴差阳错的给解决了。
**问题缘由**：后端tomcat服务默认设置表单提交数据大小上限为2M，大于2M就会报错。
**解决办法**：后端大神把server.xml中maxPostSize的值改成-1后解决。
参考资料：[blog.csdn.net/w1875690157…](http://www.javashuo.com/article/p-yyhaqsuz-mx.html)webpack

# 关于http get和form表单post提交数据大小限制

关于http get提交数据上限，以前只知道数据上限差很少是几kb大小，具体为何却没有了解java

httpget是经过url来传递数据，url不存在上限的问题，http协议也没有对utl长度作出限制，可是浏览器以及web服务器会对url长度作出限制，这个长度大小因浏览器以及服务器的不一样而不一样，通常在几kb以内。web

关于form表达提交数据大小限制，由于平时都没有提交过太大的数据，还真没有注意过这个，通常来讲post提交数据是没有大小限制的，可是tomcat默认设置表单提交数据大小上限为2m，数据大于2m,java后台将接收不到数据，解决办法是修改tomcat的server.xml中maxPostSize的值，将其设置为0即为无上限，7.0 以上版本 maxPostSize 设置为 -1面

## 正确导出图片格式

这个项目首页基本是由图片堆砌成的，一开始切出来的图(默认.png)压缩后在400k-1.3M之间。一开始还觉得PSD素材有问题。直到项目最后才闪回，想起图片格式的知识点，改导出成.jpg格式后压缩出来的图片基本控制在100K之内了。具体的.png.jpg这些图片格式的知识有兴趣的本身查。ios

## 使用html2canvas生成的海报不显示图片

**问题缘由**：引入的图片资源路径跨域形成的。
**解决办法**：我先是按照官方给的那个php的方案弄的，未能解决。最后舔着脸让后端大佬把图片资源目录挪到我web服务目录下给解决的。

```
//安装
npm install --save html2canvas
//引入
import html2canvas from "html2canvas";
//使用
const myPosterWrap = document.getElementById("posterWrap");
html2canvas(myPosterWrap).then(canvas => {
    this.posterSrc = canvas.toDataURL("image/png");
    this.uploadPosterImg(this.posterSrc);
});
```

## css黑科技之放置指定比例的图片

就是把不定宽图片按指定比例显示，直接上码（1：1.25）。

```
//html
<div class="poster-img-wrap">
    <div class="poster-img-place"></div>
    <img class="poster-img" :src="user.picAddress" alt="">
</div>
复制代码
//less
.poster-img-wrap {
    position: absolute;
    top: 28%;
    left: 0;
    right: 0;
    width: 80%;
    margin: 0 auto;
    .poster-img-place {
        width: 100%;
        padding-top: 125%;
    }
    .poster-img {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
}
```

## 、ios页面加载不全不能滚动

**问题描述** ：ios从首页进入，跳转其余页面再后退到首页，首页只显示一屏内容且没法滚动。 **问题缘由**：在于ios浏览器内核的别致渲染逻辑：它会预先找到相应的overflow: scroll元素，若是子元素高度高于父元素，则创建原生的scrollView实现滚动。问题就出如今这个“预先”上，它预先获取的高度并非子元素渲染后的真实高度。 **解决办法**：给设置了滚动的#app元素下的子元素p-index设置min-height: calc(100% + 1px);

```
#app {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: scroll;
  -webkit-overflow-scrolling: touch;
}
.p-index{
   min-height: calc(100% + 1px);
}
复制代码
```

其实能解决全靠这篇博文，这里就不赘述了。[blog.csdn.net/nongweiyila…](http://www.javashuo.com/link?url=https://blog.csdn.net/nongweiyilady/article/details/83039868)

