# X5同层播放器应用实践

video元素是比较特别的，早期无论是在iOS还是Android的浏览器中，它都位于页面的最顶层，无法被遮挡。后来，这个问题在iOS下得到了解决。但是对Android的大部分浏览器来说，问题仍然存在。X5是腾讯基于Webkit开发的浏览器内核，应用于Android端的微信、QQ、QQ浏览器等应用。它提供了一种名叫「**同层播放器**」的特殊video元素以解决遮挡问题。

## 简单使用

只要给普通的video元素加上X5的自定义属性 **x5-video-player-type** ，就可以调用同层播放器。

```html
<div class="player">
    <video id="video" class="video" controls="controls" playsinline x5-video-player-type="h5">
        <source src="video.mp4" />
    </video>
</div>

body {
    margin: 0;
    background: #000;
}
.video {
    width: 100%;
}
```

点击播放后，页面会瞬间拉伸（体验有点差），然后就进入了全屏状态，视频默认在中间位置：

![img](https://pic1.zhimg.com/80/v2-eb869701403674a55da0f73d7f1600b4_720w.jpg)

## 调整位置

在全屏状态下，调整视频位置的通用做法是：把video元素的尺寸设成**满屏**，再通过 **object-position** 样式属性控制视频内容的位置。

CSS：

```css
.fullscreen .video {
    object-position: center top;
}
```

JS：

```js
var player = document.getElementById('video');
var isFullScreen;

// 进入全屏，设置状态
player.addEventListener('x5videoenterfullscreen', function() {
    isFullScreen = true;
    // 在body上添加样式类以控制全屏状态下的页面布局
    document.body.classList.add('fullscreen');
}, false);

// 退出全屏时，清空状态
player.addEventListener('x5videoexitfullscreen', function() {
    isFullScreen = false;
    document.body.classList.remove('fullscreen');
    player.style.width = player.style.height = '';
}, false);

// 同层播放器进入全屏状态会导致窗口resize，但退出全屏不会
window.addEventListener('resize', function() {
    if (isFullScreen) {
        // 设为屏幕尺寸
        player.style.width = window.screen.width + 'px';
        player.style.height = window.screen.height + 'px';
    }
}, false);
```

**注意：**由于resize事件可以以较高的速率触发, 因此事件处理程序不应该执行计算开销很大的操作 (如 DOM 修改)，需要进行 debounce 等方式来限制执行频率。。相反, 建议使用[requestAnimationFrame](https://developer.mozilla.org/en-US/docs/DOM/window.requestAnimationFrame), [setTimeout](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setTimeout) or [customEvent](https://developer.mozilla.org/en-US/docs/Web/API/CustomEvent)。

在 [JavaScript](http://c.biancheng.net/js/) 中，resize 事件是在浏览器窗口被重置时触发的，如当用户调整窗口大小，或者最大化、最小化、恢复窗口大小显示时触发 resize 事件。利用该事件可以跟踪窗口大小的变化以便动态调整页面元素的显示大小。



效果如下方左图所示，可见，此时视频距离顶部尚有一些距离。这个问题与 **x5-video-player-fullscreen** 属性性有关。如果不声明这个属性，原标题栏的占位不会分配给页面，而是平均分成上下两块，分别位于窗口顶部和底部。因此，视频无法顶到最上方。

![img](https://pic3.zhimg.com/80/v2-49d1e3069a78b0c50b00e98fe3d35702_720w.jpg)

补充 x5-video-player-fullscreen 属性后，问题就解决了（上方右图）：

```html
<video id="video" class="video" controls="controls" playsinline x5-video-player-type="h5" x5-video-player-fullscreen="true">
    <source src="video.mp4" />
</video>
```

大家还可以发现，顶部有一层黑色渐变（上图不太明显，可以看下文的图）以及两个按钮。据官方文档所述，这些都是无法移除的。

## 全屏状态下的布局

实际业务中，页面多半不会只有一个视频这么简单。

首先是在视频之前加上标题栏：

```html
<header id="header" class="header">标题栏</header>
<div class="player">
    <video id="video" class="video" controls="controls" playsinline x5-video-player-type="h5" x5-video-player-fullscreen="true">
        <source src="video.mp4" />
    </video>
</div>
```

然而，点击播放进入全屏状态后，标题栏就消失了，其实它是被视频挡住了。既然同层播放器是可以被遮挡的，那可以试试绝对定位：

```css
.fullscreen .header {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 9999;
}
```

![img](https://pic4.zhimg.com/80/v2-bb8ca4add42978ba7e5dda6b95ba4ec7_720w.jpg)

此时视频内容被遮挡，所以要将其下移（上方右图）：

```css
.fullscreen .video {
    object-position: center 1.14rem;
}
```

接下来在视频之后添加其他页面元素，常规做法是限制视频区域高度，再进行后面的布局。但由于video元素本身在全屏状态下的宽高必须设成满屏，所以只能通过它的父元素（div.player）限制它的占位：

CSS：

```css
.player {
    height: 4.22rem;
}
.fullscreen .player {
    /* 全屏状态下重设高度，勿忘 */
    height: 5.36rem; /* 4.22 + 1.14 */
}
.video {
    width: 100%;
    height: 100%;
}
.main {
    box-sizing: border-box;
    padding: 0.3rem;
    height: 5rem;
    background: #fff;
}
```

HTML：

```html
<header id="header" class="header">...</header>
<div class="player">...</div>
<div id="main" class="main">这里是其他内容</div>
```

而div.main本身是不需要做任何特殊处理的。然而，此时又出现了一个新问题：进入全屏状态后，视频控制栏不见了。原因是，video元素的高度为屏幕高度，控制栏位于屏幕最底端，而div.player又限制了高度，导致video元素超出的区域被隐藏，自然就看不到控制栏了。幸好，我们还可以通过**伪元素选择器**修改控制栏的样式：

```css
.fullscreen .player {
    position: relative;
    height: 5.36rem;
}
.fullscreen .video::-webkit-media-controls {
    position: absolute;
    bottom: 0;
}
```

通过使控制栏相对于div.player定位，就可以让它回到视频画面的底端了。最终效果如下图所示：

![img](https://pic4.zhimg.com/80/v2-320759330de4fd85e1ddd74f6f3544af_720w.jpg)

综上所述，在**全屏状态下，video元素之前的元素需要做布局调整，而在其后的元素则不需要**。

**建议屏蔽原生控制栏**，自行开发一个控制栏。

## 视频全屏的实现

**x5-video-orientation** 属性。它决定了同层播放器进入全屏状态后，当前窗口是横屏还是竖屏（前文的所有描述中，都是竖屏的情况）。并且，它是可以动态设置的。

如果把 x5-video-orientation 设成横屏，再把页面上的其他元素隐藏掉，就跟全屏无异了。具体实现如下：

JS：

```js
var isLandscape;

// 点击其他内容区域，进入全屏
var main = document.getElementById('main');
main.addEventListener('click', function() {
    // 同层播放器进入全屏状态之后，才能让视频全屏
    if (!isFullScreen) { return; }
    // 修改 x5-video-orientation
    player.setAttribute('x5-video-orientation', 'landscape');

    isLandscape = true;
}, false);

// 检测窗口方向改变，修改布局
window.addEventListener('orientationchange', function() {
    if (isLandscape) {
        document.body.classList.add('landscape');
        var width = window.screen.width;
        var height = window.screen.height;
        player.style.width = width + 'px';
        player.style.height = height + 'px';
        
    }
}, false);
```

CSS：

```css
.landscape .header { display: none; }
.landscape .video { object-position: center center; }
.landscape .main { display: none; }
```

要强调的是，从修改 x5-video-orientation 到窗口方向改变，需要一定的时间才能完成。因此，不要在修改之后马上调整布局，而是要监听 window 的 orientationchange 事件，在事件回调中调整布局。此外，还要在退出全屏时清空相关状态：

```js
player.addEventListener('x5videoexitfullscreen', function() {
    isFullScreen = false;
    isLandscape = false;
    document.body.classList.remove('fullscreen', 'landscape');
    player.style.width = player.style.height = '';
}, false);
```

最终效果如下（顶部的阴影和两个按钮仍然无法干掉）：

![img](https://pic2.zhimg.com/80/v2-d2cb59b708813b2805e51b4ce48cd149_720w.jpg)

#### 事件回调

#### x5videoenterfullscreen进入全屏通知

支持版本: TBS中从>=036900开始支持，QB中是>=7.2开始支持

x5videoenterfullscreen: 表示播放器进入全屏状态

**通过JS监听事件**

```jsx
myVideo.addEventListener("x5videoenterfullscreen", function(){

  alert("player enterfullscreen");

})
```

#### x5videoexitfullscreen退出全屏通知

x5videoexitfullscreen: 表示播放器退出了全屏状态

使用方法与x5videoenterfullscreen类似