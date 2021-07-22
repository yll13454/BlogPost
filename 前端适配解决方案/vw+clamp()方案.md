计算公式：

```text
像素缩放值（采用像素单位值）= 设计稿理想尺寸像素值  x  当前视窗宽度（100vw） / 设计稿尺寸（理想设计稿宽度）
```

打个比方，设计稿的宽度是 750px，某个元素的字号在设计稿上 24px ，那么：

```text
font-size: calc(24 * 100vw / 750)
```

如果把区间值整进来：

```text
font-size: calc(24 * clamp(320px, 100vw, 1440px) / 750)
```

可以借助 CSS 自定义属性，让它变得更简单一点：

```
:root { 
  --ideal-viewport-width: 750; 
  --current-viewport-width: 100vw; 
  --min-viewport-width: 320px; 
  --max-viewport-width: 1440px; 
  --clamped-viewport-width: clamp( 
    var(--min-viewport-width), 
    var(--current-viewport-width), 
    var(--max-viewport-width) ) 
}

.element {
   font-size: calc(24 * var(--clamped-viewport-width) / var(--ideal-viewport-width))
}
```

## Flexible方案

#### 2,Flexible配合的周边工具

##### PostCSS-px2rem

```js
plugins: {
    ...,
    'postcss-pxtorem': {
        // 750设计标准
        rootValue: 75,
        // 转换成的rem后，保留小数点后几位
        unitPrecision: 5,
        /**
        * 将会被转换的css属性列表，
        * 设置为*表示全部，['*','*position*','!letter-spacing','!font*']
        * *position* 表示所有包含 position 的属性
        * !letter-spacing 表示非 letter-spacing 属性
        * !font* 表示非font-size font-weight ... 等的属性
        * */
        propList: ['*', '!letter-spacing'],
        // 不会被转换的class选择器名，支持正则
        selectorBlackList: ['.rem-'],
        replace: true,
        // 允许在媒体查询中转换`px`
        mediaQuery: false,
        // 小于1px的将不会被转换
        minPixelValue: 1
    }
}
```

配置在当前项目根目录的`postcss.config.js`中即可

#### 3,Flexible的缺陷

#####  对iframe的使用不兼容。

#####  对高倍屏的安卓手机没做处理

如果你去研究过`lib-flexible`的源码，那你一定知道`lib-flexible`对安卓手机的特殊处理，即：一律按`dpr = 1`处理。

##### 不兼容响应式布局

##### 无法正确响应系统字体大小

## Viewport方案

#### 2 Viewport方案配合的周边工具

##### postcss-px-to-viewport

根目录下的`postcss.config.js`中即可，其基本配置项如下：

```js
plugins: {
'postcss-px-to-viewport': {
    unitToConvert: 'px',   // 需要转换的单位
    viewportWidth: 750,    // 视口宽度，等同于设计稿宽度
    unitPrecision: 5,      // 精确到小数点后几位
    /**
    * 将会被转换的css属性列表，
    * 设置为 * 表示全部，如：['*']
    * 在属性的前面或后面设置*，如：['*position*']，*position* 表示所有包含 position 的属性，如 background-position-y
    * 设置为 !xx 表示不匹配xx的那些属性，如：['!letter-spacing'] 表示除了letter-spacing 属性之外的其他属性
    * 还可以同时使用 ! 和 * ，如['!font*'] 表示除了font-size、 font-weight ...这些之外属性之外的其他属性名头部是‘font’的属性
    * */
    propList: ['*'],
    viewportUnit: 'vw',    // 需要转换成为的单位
    fontViewportUnit: 'vw',// 需要转换称为的字体单位
    /**
    * 需要忽略的选择器，即这些选择器对应的属性值不做单位转换
    * 设置为字符串，转换器在做转换时会忽略那些选择器中包含该字符串的选择器，如：['body']会匹配到 .body-class，也就意味着.body-class对应的样式设置不会被转换
    * 设置为正则表达式，在做转换前会先校验选择器是否匹配该正则，如果匹配，则不进行转换，如[/^body$/]会匹配到 body 但是不会匹配到 .body
    */
    selectorBlackList: [],
    minPixelValue: 1,      // 最小的像素单位值
    mediaQuery: false,     // 是否转换媒体查询中设置的属性值
    replace: true,                 // 替换包含vw的规则，而不是添加回退
    /**
    * 忽略一些文件，如'node_modules'
    * 设置为正则表达式，将会忽略匹配该正则的所有文件
    * 如果设置为数组，那么该数组内的元素都必须是正则表达式
    */
    exclude: [],
    landscape: false,      // 是否自动加入 @media (orientation: landscape)，其中的属性值是通过横屏宽度来转换的
    landscapeUnit: 'vw',   // 横屏单位
    landscapeWidth: 1334   // 横屏宽度
}
```

可以在`selectorBlackList`选项中设置一些关键词或正则，来避免对这些指定的选择器做转换，如`selectorBlackList：['.ignore', '.hairlines']`：

```
<div class="box ignore"></div>
写CSS的时候： 
.ignore {
    margin: 10px;
    background-color: red;
}
.box {
    width: 180px;
    height: 300px;
}
.hairlines {
    border-bottom: 0.5px solid red;
}
```

转化之后：

```
.box {
    width: 24vw;
    height: 40vw;
}
.ignore {
    margin: 10px; /*.box元素中带有.ignore类名，在这个类名写的`px`不会被转换*/
    background-color: red;
}
.hairlines {
    border-bottom: 0.5px solid red;
}
```

##### Viewport Units Buggyfill

这个js库是为了兼容那些不兼容`vw`、`vh`、`vmax`、`vmin`这些viewport单位的浏览器所使用的

##### 使用方法

1\. 引入JavaScript文件

`viewport-units-buggyfill`主要有两个JavaScript文件：`viewport-units-buggyfill.js`和`viewport-units-buggyfill.hacks.js`。你只需要在你的HTML文件中引入这两个文件。比如在Vue项目中的`index.html`引入它们：

```
<script src="//g.alicdn.com/fdilab/lib3rd/viewport-units-buggyfill/0.6.2/??viewport-units-buggyfill.hacks.min.js,viewport-units-buggyfill.min.js"></script>
```

###### 2\. 在HTML文件中调用viewport-units-buggyfill

在html文件中引入`polyfill`的位置之后，需要手动调用下 `viewport-units-buggyfill`:

```
<script>
  window.onload = function () {
    window.viewportUnitsBuggyfill.init({
      hacks: window.viewportUnitsBuggyfillHacks
    });
}
</script>  
```

###### 3\. 结合使用postcss-viewport-units

具体的使用。在你的`CSS`中，只要使用到了viewport的单位地方，需要在样式中添加`content`：

```
.my-viewport-units-using-thingie {
  width: 50vmin;
  height: 50vmax;
  top: calc(50vh - 100px);
  left: calc(50vw - 100px);
  /* hack to engage viewport-units-buggyfill */
  content: 'viewport-units-buggyfill; width: 50vmin; height: 50vmax; top: calc(50vh - 100px); left: calc(50vw - 100px);';
}  
```

这可能会令你感到恶心，而且我们不可能每次写`vw`都去人肉的计算。特别是在我们的这个场景中，我们使用了[postcss-px-to-viewport](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Fevrone%2Fpostcss-px-to-viewport)这个插件来转换`vw`，更无法让我们人肉的去添加`content`内容。

这个时候就需要前面提到的[postcss-viewport-units](https://link.segmentfault.com/?url=https%3A%2F%2Fgithub.com%2Fspringuper%2Fpostcss-viewport-units)插件。这个插件将让你无需关注`content`的内容，插件会自动帮你处理。比如插件处理后的代码：

```
.test {
    padding: 3.2vw;
    margin: 3.2vw auto;
    background-color: #eee;
    text-align: center;
    font-size: 3.73333vw;
    color: #333;
    content: "viewport-units-buggyfill; padding: 3.2vw; margin: 3.2vw auto; font-size: 3.73333vw";
}  
```

配置这个插件也很简单，只需要和配置`postcss-px-to-viewport`一样，配置在项目根目录的`postcss.config.js`中即可：

```
plugins: {
  'postcss-viewport-units': {}
} 
```

##### 副作用

在我们使用了Viewport Units Buggyfill后，正如你看到的，它会在占用`content`属性，因此会或多或少的造成一些副作用。如`img`元素和伪元素的使用`::before`或`::after`。

对于img，在部分浏览器中，`content`的写入会造成图片无法正常展示，这时候需要全局添加样式覆盖：

```
img {
    content: normal !important;
} 
```

对于`::before`等伪元素，就算是在里面使用了vw单位，Viewport Units Buggyfill对其并不会起作用，

