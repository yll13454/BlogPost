```
/* 横向滚动容器 */
.scroll-wrapper {
    width: 100%;
    overflow-x: auto;
    height: 22px;
    white-space: nowrap;
    display: flex;
    -webkit-overflow-scrolling: touch;
}
.scroll-wrapper span {
    display: block;
    padding: 0 4px;
    margin: 0 10px;
    height: 20px;
    line-height: 20px;
    font-size: 14px;
    border: 1px solid #3884ff;
    border-radius: 20px;
    color: #3884ff;
}
/* 滚动条最外面轨道样式 :: 前面保留一个空格*/
.scroll-wrapper ::-webkit-scrollbar {
    width: 2px; /* 对纵向滚动条起作用*/
    height: 2px; /* 对横向滚动条起作用*/
    background: gray;
    display: none; /* 不显示滚动条 */
}

/* 滚动条最外面滑块样式 :: 前面保留一个空格*/
.scroll-wrapper ::-webkit-scrollbar-thumb {
    background: pink;
    border-radius: 2px;
    width: 2px; /* 对纵向滚动条起作用*/
    height: 2px; /* 对横向滚动条起作用*/
}
```

html结构

```
<div class="scroll-wrapper">
    <span>awesome</span>
    <span>awesome</span>
    <span>awesome</span>
    <span>awesome</span>
    <span>awesome</span>
    ...
</div>12345678
```

::-webkit-scrollbar 双冒号前记得保留一个空格，否则ios上隐藏滚动条在safari上无效**重点内容**



```
  /* 定义滚动条样式 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
  background-color: rgba(240, 240, 240, 1);
}
 
/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  box-shadow: inset 0 0 0px rgba(240, 240, 240, .5);
  border-radius: 10px;
  background-color: rgba(240, 240, 240, .5);
}
 
/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  box-shadow: inset 0 0 0px rgba(240, 240, 240, .5);
  background-color: rgba(240, 240, 240, .5);
}
```





### 时时刻刻显示滚动条

{
overflow-y:scroll;
}

可以参考这个问题[点击传送](https://segmentfault.com/q/1010000005960171)

[Always show scrollbars in iPhone/Android duplicate\](https://stackoverflow.com/questions/25684620/always-show-scrollbars-in-iphone-android)



### [H5之移动端如何让滚动条不隐藏](http://bighone.com/2018/03/27/H5%E4%B9%8B%E7%A7%BB%E5%8A%A8%E7%AB%AF%E5%A6%82%E4%BD%95%E8%AE%A9%E6%BB%9A%E5%8A%A8%E6%9D%A1%E4%B8%8D%E9%9A%90%E8%97%8F/)

**滚动条常亮**。

项目框架使用React + Redux + FastClick + [Ant Design Mobile](https://mobile.ant.design/index-cn)，可惜的是移动端没有这种下拉联动的组件，GitHub也没搜到（可能没找到），就干脆自己去造了一个，这样**滚动条常亮**的需求也更能够受控。

### 利用CSS

看到这样的需求，首先想到的就是利用css来控制滚动条常亮显示。当然也很容易搜索到[这篇博客](http://blog.csdn.net/yzbben/article/details/61198969)，从中找到如下代码放入项目：

```
// 基本css代码
overflow-y: scroll; // 滚动显示
-webkit-overflow-scrolling: touch;  // 滚动回弹

/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/  
&::-webkit-scrollbar  
{  
    width: 16px;  
    height: 16px;  
    background-color: #F5F5F5;  
}  
  
/*定义滚动条轨道 内阴影+圆角*/  
&::-webkit-scrollbar-track  
{  
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);  
    border-radius: 10px;  
    background-color: #F5F5F5;  
}  
  
/*定义滑块 内阴影+圆角*/  
&::-webkit-scrollbar-thumb  
{  
    border-radius: 10px;  
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);  
    background-color: #555;  
}
```

运行，开始在电脑的模拟手机浏览器上面确实成功了，以为OK，问题很容易得到解决，可是放到手机上一看，大失所望，滚动条还是没有显示，没任何效果，咋回事，有点坑啊！只能闷着头皮，各种百度谷歌找原因，终于在[这里](https://stackoverflow.com/questions/11825156/mobile-webkit-scroll-bars)发现了问题所在。

> You can’t style the native scrollbar on mobile, and I doubt you ever will be able to, sorry.

> I can’t say why with any authority, but all phones and OS’s use different browsers and each one will have it’s own implementation of the scroll bar. A lot have taken iOS’s fading scrollbar approach too, which means there isn’t much point styling it anyway.

> This does limit you to using a custom solution really.

> the mobile safari do not support div scroll, you can use iscoll framework

原来是手机浏览器不支持scroll，只对PC端浏览器生效，推荐使用[iScroll](https://github.com/cubiq/iscroll/)框架来处理移动端的这种问题。

很开心，终于找到解决方案了，以为把这框架集成进来就可以了，但熟料这才刚刚开始，[iScroll](https://github.com/cubiq/iscroll/)中的坑比想象的要深的多啊。

### 集成iScroll

到[这里](http://iscrolljs.com/)下载iScroll，最新版本是5.1.2，集成到项目当中，将列表用iScroll包装，代码如下：

```
myScroll = new IScroll('#wrapper', {
	// 允许滑轮滚动
 	mouseWheel: true,  
 	// 显示滚动条
	scrollbars: true,
 });
```

运行，滚动条在手机端终于常亮显示了，我是用iPhone浏览器进行测试的，一切正常，没啥问题。换了台Android手机去跑，尼玛，滚动条是显示了，但是列表项**点不了啦，点击事件不响应啦！！！**

没办法，继续查资料，很容易在[这里](https://stackoverflow.com/questions/17768037/enable-click-events-in-iscroll-on-mobile-browser)发现需要在iScroll的options设置click和tap，开启默认的点击事件，代码如下：

```
myScroll = new IScroll('#wrapper', {
	// 允许滑轮滚动
 	mouseWheel: true,  
 	// 显示滚动条
	scrollbars: true,
	click: true,
	// 该项目可省略
	// 根据描述，如果不加，<a>标签点击在部分手机浏览器不会生效
	tap: true
 });
```

终于Android手机列表项可以响应点击事件了！！！

可是还没完，回到iPhone设备上测试，竟然发现iPhone又不能点击列表项了啦，不是点不了，而是需要双击才能有效，这不是玩我么，有这么坑的！

彻底的受伤害了，突然发现原生APP的手机适配虽然也繁琐，但是比这玩意儿要舒服多了啊！

不过问题还是要解决，还是得回到google的怀抱。最后还是在iScroll的issue中找到了[解决方案](https://github.com/cubiq/iscroll/issues/674)，手机浏览器全方位适配。

```
function iScrollClick(){
    if (/iPhone|iPad|iPod|Macintosh/i.test(navigator.userAgent)) return false;
    if (/Chrome/i.test(navigator.userAgent)) return (/Android/i.test(navigator.userAgent));
    if (/Silk/i.test(navigator.userAgent)) return false;
    if (/Android/i.test(navigator.userAgent)) {
        var s=navigator.userAgent.substr(navigator.userAgent.indexOf('Android')+8,3);
        return parseFloat(s[0]+s[3]) < 44 ? false : true
    }
    return false;
}

myScroll = new IScroll('#wrapper', {
 	mouseWheel: true,  
	scrollbars: true,
	click: iScrollClick(),
	tap: iScrollClick()
});
```

终于搞定。

\######分析：经过测试，将FastClick关闭后，这里click始终传ture的话，苹果手机的双击问题竟然就没有了。所以猜测当iScroll配合FastClick使用的时候，iOS端其实已经激活了iScroll的click，如果再次设置，就会出现iOS端需要双击的问题，具体内部原理搞不清楚，有时间好好研究下，所以配合FastClick使用的话记得加上iScrollClick函数对click进行判断即可。

### 总结

滚动条常亮的问题确实是解决了，但是还是有一些瑕疵，如最上面的需求图，少部分安卓手机，如Galaxy S5，Nexus 6P的浏览器点击下拉列表项后，竟然会穿透到底部内容列表Cell的事件上去，估计也是因为FastClick的原因吧，但是这个问题谷歌了很久，都没找到解决方案，如果有人知道的话，不吝赐教，灰常感谢。

既然问题发现了，对我来说是难以忍受的，这里就暂时先用了一个不太理想的方式去处理，代码放出来，应该看得懂，就不解释了。

```
// 下拉列表选择城市事件
handlerPullCellClick() {
	....
	document.prevent = true;
	setTimeout(() => {
	    document.prevent = false;
	}, 100);
}

// 内容cell点击事件
handlerContentCellClick() {
	// 禁止部分手机出现穿透效果
	if (document.prevent) return; 
	...
}
```

### 后记

这些问题相信很多童鞋都遇到过，如果有更好的解决方案的话，希望可以再评论区告知，让大家一起学习下，感谢。

另外，如果有童鞋需要这个下拉联动选择器组件代码的话，可到这里下载：[react-selector](https://github.com/bighone/react-selector)。