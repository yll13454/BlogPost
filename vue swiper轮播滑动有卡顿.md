## vue swiper在安卓内轮播滑动有卡顿，在ios内没有

swiper在安卓内轮播滑动有卡顿，在ios内没有，百度都没有找到解决方案



------


参考答案1：

-webkit-overflow-scrolling: touch; css加上试一下

## Swiper 在 iOS 13 中出现卡顿，即滑动页面后页面停顿在两个 Swiper Item 中间。

```js
//// 将下发代码放置在 HTML 中
document.body.addEventListener('touchmove', function(e) {e.preventDefault()}, {passive: false})
```



### 移动端使用e.preventDefault()禁止滚动及取消

移动端防止页面滚动代码(兼容苹果和安卓)

```jsx
// 防止下拉
function touchmove () {
    document.body.addEventListener('touchmove', function (e) {
        e.preventDefault()
    }, {passive: false})
}
```

遮罩层消失之后允许下拉

```jsx
function untouchmove () {
    document.body.addEventListener('touchmove', function (e) {
        window.event.returnValue = true
    })
}
```

在遮罩层弹出是调用`touchmove()`方法，关闭遮罩层之后调用`untouchmove()`方法，即可实现想要的效果。





## [移动Web轮播图IOS卡顿的问题](https://www.cnblogs.com/zjzhome/p/4902568.html)

HTML结构：

```html
<div id='slider' class='swipe'>
  <div class='swipe-wrap'>
    <div></div>
    <div></div>
    <div></div>
  </div>
</div>
```

CSS样式：

```css
.swipe {
  overflow: hidden;
  visibility: hidden;
  position: relative;
}
.swipe-wrap {
  overflow: hidden;
  position: relative;
}
.swipe-wrap > div {
  float:left;
  width:100%;
  position: relative;
}
```

JavaScript代码：

```js
window.mySwipe = new Swipe(document.getElementById('slider'), {
  startSlide: 2,
  speed: 400,
  auto: 3000,
  continuous: true,
  disableScroll: false,
  stopPropagation: false,
  callback: function(index, elem) {},
  transitionEnd: function(index, elem) {}
});
```

**解决方案**

```css
.swipe {
  overflow: hidden;
  visibility: hidden;
  position: relative;
}
.swipe-wrap {
  overflow: hidden;
  position: relative;
    //加载代码
  -webkit-backface-visibility: hidden;
  backface-visibility: hidden; 
  -webkit-transform-style: preserve-3d;
  transform-style: preserve-3d;
}
.swipe-wrap > div {
  float:left;
  width:100%;
  position: relative;
}
```

## (Swiperjs插件轮播滑动卡顿优化)[https://juejin.cn/post/7016993922870312967]

轮播个数大于20



## ios H5 页面滑动卡顿_Young_Gao的博客-程序员秘密



在滚动容器内加`-webkit-overflow-scrolling: touch`

但这个方案有一个问题，在页面内具有多个`overflow：auto`的情况下，那些具有 绝对定位（absolute, fixed） 属性的元素，也会跟着滚动。

```css
body {overflow-x: hidden}
```

即，给 body 元素添加overflow-x：hidden。然后在滚动容器内加`overflow-y: auto` 

```css
main{
height:100%;
overflow-y:scroll;
-webkit-overflow-scrolling:touch;
}
```

