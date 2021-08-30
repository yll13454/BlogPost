1.安装依赖



```undefined
npm install postcss-pxtorem -D
```

2.设置规则（更改postcss.config.js,该文件为使用vue-cli3自动创建的文件）



```java
module.exports = {
  plugins: {
    'autoprefixer': {
      browsers: ['Android >= 4.0', 'iOS >= 7']
    },
    'postcss-pxtorem': {
      rootValue: 16,//结果为：设计稿元素尺寸/16，比如元素宽320px,最终页面会换算成 20rem
      propList: ['*']
    }
  }
}
```

> 操作到这里移动端自动适配了吗，当然不能，目前只是将px单位转换成rem,移动端适配还差最后一步,初接触rem理解起来有点懵，后来发现了一个好理解的方法，下面来分享一下。

现在我们作一个实验，你可以新建一个网页，并写入如下代码：



```jsx
<div class="test">
    <p class="hello">Hello</p>
</div>
```

然后给html一个基本的样式:



```css
.test{
    width:320px;
    height:160px;
    background-color: bisque;
    text-align: center；
}
.hello{
    color:red;
}
```

上边我们使用了还是传统的使用px作为单位，我们在移动端调试模式iphone5环境查看一下。会发现div的宽度是正好的，我们再查看一下字体，发现大小是16px。
 我们现在可以把CSS中的px单位换成rem单位来进行测试。因为html根元素的字体大小是16px，那么换成rem单位，直接除以16就好。



```css
.test{
    width:20rem;
    height:10rem;
    background-color: bisque;
    text-align: center；
}
.hello{
    color:red;
    font-size:1rem;
}
```

明白了REM的原理后，我们就可以使用这个特点来进行适应布局了，这也是现在比较主流的移动端web适配方案。src目录下，新建 libs/rem.js 输入如下代码



```jsx
// 设置 rem 函数
function setRem () {

    // 320 默认大小16px; 320px = 20rem ;每个元素px基础上/16
    let htmlWidth = document.documentElement.clientWidth || document.body.clientWidth;
//得到html的Dom元素
    let htmlDom = document.getElementsByTagName('html')[0];
//设置根元素字体大小
    htmlDom.style.fontSize= htmlWidth/20 + 'px';
}
// 初始化
setRem();
// 改变窗口大小时重新设置 rem
window.onresize = function () {
    setRem()
}
```

在main.js中引入rem.js



```dart
import './libs/rem.js';
```

最后刷新页面看下，就会发现原本用px的单位已经自动换算成了rem;



[vue3.0+vite 使用 postcss-pxtorem 实现移动自适应（px转rem）](https://blog.csdn.net/weixin_42164539/article/details/113999807)

```js
//postcss.config.js
module.exports = {
    plugins: {
        autoprefixer: {
            overrideBrowserslist: [
                "Android 4.1",
                "iOS 7.1",
                "Chrome > 31",
                "ff > 31",
                "ie >= 8",
                "last 10 versions", // 所有主流浏览器最近10版本用
            ],
            grid: true,
        },
        'postcss-pxtorem': {
            rootValue: 37.5,
            propList: ['*'],
            unitPrecision: 5
        }
    }
}
```

这里只实现了 `px`转`rem`，还要安装 `amfe-flexible`

> npm i amfe-flexible -D

在`main.ts`文件中 import 一下就好可以了

> import ‘amfe-flexible/index.js’

可能会出现下面情况
`[vite] Internal server error: Loading PostCSS Plugin failed: Cannot find module 'autoprefixer'`
![在这里插入图片描述](media/20210223183508808.png)
这时候尝试安装 `autoprefixer`就可以了

> npm i autoprefixer

