## 背景

> 目前遇到这么一个问题：我有一个可以向下展开的下来菜单，菜单初始高度大于300px左右，没有超过手机屏幕高度，当展开的时候如果超过手机屏幕高度时让父元素出现滚动条滚动，就是说内容的高度是动态的。

## 描述

此前我通过js动态获取屏幕高度，并设置为父元素的max-height。然后设置父元素over[flow](https://so.csdn.net/so/search?q=flow)-y: scroll;

起初，我大概的代码布局如下：

```html
 <div className='。' style={{ maxHeight: this.state.viewPortHeihgt + "px" }}>
     <Lists>
       .
       .
       .
     </Lists>
   </div>
1234567
```

其中Lists中的内容的高度是动态的。
css如下：

```css
 .small-nav{
     overflow-y: scroll;
     overflow-x: hidden;
   }
1234
```

此方法在andriod和pc上运行正常，但是在ios上就出现卡出不能滑动的现象。

## 分析

我们知道，页面的渲染流程分为以下几个步骤：

1. 构建DOM tree
2. 构建CSS Rule tree
3. 根据DOM tree和CSS tree来构建render tree
4. 根据render tree计算页面的layout
5. render页面

safari浏览器在构建render tree的时候，会预先找到相应的overflow: [scroll](https://so.csdn.net/so/search?q=scroll)元素，在计算页面layout的时候，会计算父元素的高度与子元素的高度，若子元素高于父元素，则在render页面时为其建立一个原生的scrollView。当子元素的高度小于父元素的高度时，safari不会给父元素一个原生的scrollView。

这里我Lists中的内容初始是小于父元素.small-nav的高度的，所以在ios的解释中不会给父元素添加一个scrollView。

## 解决

针对这种情况，我们可以设置让浏览器一开始就给父元素增加scrollView，当我们的内容撑开，高过父元素的时候，就可以进行滑动。

代码如下：

```html
 <div className='small-nav-outer' style={{ height: this.state.viewPortHeihgt + "px" }}>
   <div className='small-nav-inner'>
     <Lists>
       .
       .
       .
     </Lists>
   </div>
 </div>
123456789
.small-nav-outer{
  -webkit-overflow-scrolling: touch;
   overflow-y: scroll;
   overflow-x: hidden;
 }
 .small-nav-inner{
   min-height: calc(100% + 1px);
 }
12345678
```

代码中可以看出，我在父元素的内部添加一个一个包裹的div，.small-nav-inner，让他的高度高于父元素1px，然后给父元素添加 -webkit-overflow-scrolling: touch;属性，这样可以令一开始的时候就添加一个scrollView。

如此，ios上的兼容性问题得以解决。

以上内容参考大神博文：
https://segmentfault.com/a/1190000016408566