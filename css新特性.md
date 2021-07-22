### 自定义属性

自定义属性也和普通属性一样具有级联性，申明在 :root 下的时候，在全文档范围内可用，而如果是在某个元素下申明自定义属性，则只能在它及它的子元素下才可以使用。

自定义属性必须通过 `--x` 的格式申明，比如：--theme-color: red; 使用自定义属性的时候，需要用 var 函数。比如：

```css
<!-- 定义自定义属性 -->
:root {
    --theme-color: red;
}

<!-- 使用变量 -->
h1 {
    color: var(--theme-color);
}
```



### 清除浮动

因为浮动元素会脱离正常的文档流，并不会占据文档流的位置，所以如果一个父元素下面都是浮动元素，那么这个父元素就无法被浮动元素所撑开，这样一来父元素就丢失了高度，这就是所谓的浮动造成的父元素高度坍塌问题。

#### 1,BFC 清除浮动

计算 BFC 高度的时候浮动子元素的高度也将计算在内，给父元素设置overflow: auto 来简单的实现 BFC 清除浮动，但是为了兼容 IE 最好用 overflow: hidden。

通过 overflow: hidden 来清除浮动并不完美，当元素有阴影或存在下拉菜单的时候会被截断，所以该方法使用比较局限。

#### 2,通过 clear 清除浮动

 ```css
 .clearfix {
     zoom: 1;
 }
 .clearfix::after {
     content: "";
     display: block;
     clear: both;
 }
 ```

![img](media/05edf023dd564a2f8d11ab47c3d56361~tplv-k3u1fbpfcp-zoom-1.image)

##### clear如何清除浮动？

clear属性不允许被清除浮动的元素的左边/右边挨着浮动元素，底层原理是在被清除浮动的元素上边或者下边添加足够的清除空间。

**注意**:我们是通过在别的元素上清除浮动来实现撑开高度的， 而不是在浮动元素上。

```css
// 现代浏览器clearfix方案，不支持IE6/7
.clearfix:after {
    display: table;
    content: " ";
    clear: both;
}

// 全浏览器通用的clearfix方案
// 引入了zoom以支持IE6/7
.clearfix:after {
    display: table;
    content: " ";
    clear: both;
}
.clearfix{
    *zoom: 1;
}

// 全浏览器通用的clearfix方案【推荐】
// 引入了zoom以支持IE6/7
// 同时加入:before以解决现代浏览器上边距折叠的问题
.clearfix:before,
.clearfix:after {
    display: table;
    content: " ";
}
.clearfix:after {
    clear: both;
}
.clearfix{
    *zoom: 1;
}
```

### 消除浏览器默认样式

区别于 reset.css，Normalize.css 有如下特点：

- reset.css 几乎为所有标签都设置了默认样式，而 Normalize.css 则是有选择性的保护了部分有价值的默认值；
- 修复了很多浏览器的 bug，而这是 reset.css 没做到的；
- 不会让你的调试工具变的杂乱，相反 reset.css 由于设置了很多默认值，所以在浏览器调试工具中往往会看到一大堆的继承样式，显得很杂乱；
- Normalize.css 是模块化的，所以可以选择性的去掉永远不会用到的部分，比如表单的一般化；
- Normalize.css 有详细的说明文档；

### 水平垂直居中

#### 单行的文本、inline 或 inline-block 元素

**水平居中**

此类元素需要水平居中，则父级元素必须是块级元素(`block level`)，且父级元素上需要这样设置样式：

```css
.parent {
    text-align: center;
}
```

**垂直居中**

方法一：通过设置上下内间距一致达到垂直居中的效果：

方法二：通过设置 `height` 和 `line-height` 一致达到垂直居中：

#### 固定宽高的块级盒子

**方法一：absolute + 负 margin**

![img](media/8cc55312dcda4924abdfc7a8bc20db26~tplv-k3u1fbpfcp-zoom-1.image)

**方法二：absolute + margin auto**

![img](media/f593f38b4eb74fb4a8a1b95cc0476e87~tplv-k3u1fbpfcp-zoom-1.image)

**方法三：absolute + calc**

![img](media/4d36b58ad7e04861b4667e4f5f69ae19~tplv-k3u1fbpfcp-zoom-1.image)

#### 不固定宽高的块级盒子

这里列了 6 种方法，参考了[颜海镜](https://link.juejin.cn/?target=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000016389031) 写的文章 ，其中的两种 line-height 和 writing-mode 方案看后让我惊呼：还有这种操作？学到了学到了。

**方法一：absolute + transform**

![img](media/e69e2c2e35f74ae6a0c38949d627798e~tplv-k3u1fbpfcp-zoom-1.image)

**方法二：line-height + vertical-align**

![img](media/2cca3c98ae284d1aa6db063739df6e2a~tplv-k3u1fbpfcp-zoom-1.image)

**方法三：writing-mode**

![img](media/a345ddcd7bad462eb390b4688cb29c31~tplv-k3u1fbpfcp-zoom-1.image)

**方法四：table-cell**

![img](media/317b0670c9fe4c90bb8b77f3f90d8c79~tplv-k3u1fbpfcp-zoom-1.image)

**方法五：flex**

![img](media/070f1c7a61ec4cbd8aa2d9b000f8dcd4~tplv-k3u1fbpfcp-zoom-1.image)

**方法六：grid**

![img](media/1b509172aabc4a3580c3eb57953755f5~tplv-k3u1fbpfcp-zoom-1.image)

### 常用布局

#### 两栏布局（边栏定宽主栏自适应）

针对以下这些方案写了几个示例： [codepen demo](https://link.juejin.cn/?target=https%3A%2F%2Fcodepen.io%2Fbulandent%2Fpen%2FJjbqxbM)

**方法一：float + overflow（BFC 原理）**

![img](media/4f90447388404ebd9a9c238317e81e23~tplv-k3u1fbpfcp-zoom-1.image)

**方法二：float + margin**

![img](media/544ac1c6ec884ff18582faa348b69c70~tplv-k3u1fbpfcp-zoom-1.image)

**方法三：flex**

![img](media/c8866bac770443b39c806150cdcc8a7c~tplv-k3u1fbpfcp-zoom-1.image)

**方法四：grid**

![img](media/688c3b7599614081b1df80a50ec2965f~tplv-k3u1fbpfcp-zoom-1.image)

#### 三栏布局（两侧栏定宽主栏自适应）

针对以下这些方案写了几个示例： [codepen demo](https://link.juejin.cn/?target=https%3A%2F%2Fcodepen.io%2Fbulandent%2Fpen%2FabBrXrj)

**方法一：圣杯布局**

![img](media/2c3df7e23b8d4fbd96eeeccb4fc99215~tplv-k3u1fbpfcp-zoom-1.image)

**方法二：双飞翼布局**

![img](media/488a7974931f46d687075c30468286f1~tplv-k3u1fbpfcp-zoom-1.image)

**方法三：float + overflow（BFC 原理）**

![img](media/62fb7453443344149fcf27d551e63075~tplv-k3u1fbpfcp-zoom-1.image)

**方法四：flex**

![img](media/4c00727559ed4833892b01d12193c621~tplv-k3u1fbpfcp-zoom-1.image)

**方法五：grid**

![img](media/7e8d2ef98c4c4d718a10f91984c57e71~tplv-k3u1fbpfcp-zoom-1.image)


作者：大海我来了
链接：https://juejin.cn/post/6941206439624966152
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。