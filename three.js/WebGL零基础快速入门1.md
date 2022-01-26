# 第一章 WebGL零基础快速入门

### 渲染管线流程图

![img](media/webgl25a.png)

# WebGL绘制一个点

## HTMLCanvasElement.getContext()

> ```js
> var ctx = canvas.getContext(contextType);
> var ctx = canvas.getContext(contextType, contextAttributes);
> ```
>
> **`HTMLCanvasElement.getContext()`** 方法返回`canvas` 的上下文，如果上下文没有定义则返回 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null) .

### [返回值](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/getContext#返回值)

[`RenderingContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/RenderingContext) 返回值是下列之一：

- [`CanvasRenderingContext2D`](https://developer.mozilla.org/zh-CN/docs/Web/API/CanvasRenderingContext2D) 为 `"2d"`,
- [`WebGLRenderingContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGLRenderingContext) 为`"webgl"` 和`"experimental-webgl"`,
- [`WebGL2RenderingContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL2RenderingContext) 为`"webgl2"` 和`"experimental-webgl2"`, 或者
- [`ImageBitmapRenderingContext`](https://developer.mozilla.org/zh-CN/docs/Web/API/ImageBitmapRenderingContext) 为`"bitmaprenderer"`.

如果 `*contextType*` 不是上述之一，返回[`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null).



### 创建Canvas画布

创建一个`Canvas`画布，用于显示WebGL的渲染结果，canvas元素和div等元素一样是HTML的一个元素，只是`Canvas`画布具有2D和3D绘图功能。

```HTML
<!--canvas标签创建一个宽高均为500像素，背景为蓝色的矩形画布-->
<canvas id="webgl" width="500" height="500" style="background-color: blue"></canvas>
```

通过JavaScript获取上面创建的Canvas元素返回一个Canvas对象。

```JavaScript
//通过getElementById()方法获取canvas画布对象
var canvas= document.getElementById('webgl')
```

## Canvas对象方法`.getContext()`

HTML的Canvas元素提供了2D和3D绘图两种功能，平时程序员之间交流所说的Canvas一词就是指Canvas的2D绘图功能，通过Canvas元素实现的3D绘图功能，也就是所谓的WebGL，或者说WebGL依赖于Canvas元素实现。

执行`canvas.getContext('2d')`返回对象具有一系列绘制二维图形的方法，比如绘制直线、圆弧等API。

> 执行`canvas.getContext('webgl');`返回对象具有一系列绘制渲染三维场景的方法，也就是WebGL API。

```js
//通过方法getContext()获取WebGL上下文
var gl=canvas.getContext('webgl');
...
//调用WebGL API绘制渲染方法drawArrays
gl.drawArrays(gl.POINTS,0,1);
...
```

## 着色器语言GLSL ES

下面代码中的两个字符串`vertexShaderSource`和`fragShaderSource`是WebGL的着色器代码，着色器代码通过着色器语言GLSL ES编写

 与OpenGL API相配合的是着色器语言GLSL，与OpenGL ES API、WebGL API相互配合的是着色器语言GLSL ES。OpenGL标准应用的是客户端 OpenGL ES应用的是移动端，WebGL标准应用的是浏览器平台。

顶点着色器定义了顶点的渲染位置和点的渲染像素大小

```JavaScript
//顶点着色器源码
var vertexShaderSource = '' +
    'void main(){' +
    //给内置变量gl_PointSize赋值像素大小
    '   gl_PointSize=20.0;' +
    //顶点位置，位于坐标原点
    '   gl_Position =vec4(0.0,0.0,0.0,1.0);' +
    '}';
```

片元着色器定义了点的渲染结果像素的颜色值

```JavaScript
//片元着色器源码
var fragShaderSource = '' +
    'void main(){' +
    //定义片元颜色
    '   gl_FragColor = vec4(1.0,0.0,0.0,1.0);' +
    '}';
```

gl_PointSize、gl_Position、gl_FragColor都是内置变量

> 通过程序可以看出来顶点着色器源码`vertexShaderSource`、片元着色器源码`fragShaderSource`，都是只有一个主函数main，也就是入口函数。

给内置变量`gl_Position`赋值`vec4(0.0,0.0,0.0,1.0)`，也就是设置顶点位置坐标，vec4代表的是一种数据类型， 在这里可以理解为vec4()是一个可以构造出vec4类型数据的构造函数，前三个参数表示顶点坐标值xyz。

给内置变量`gl_FragColor`赋值`vec4(1.0,0.0,0.0,1.0)`，也就是设置会在屏幕上显示的像素的颜色，vec4()构造函数 前三个参数，表示颜色RGB值，最后一个参数是透明度A。在WebGL着色器中颜色值使用[0,1]区间表示。

### WebGL API

一句话来描述，WebGL API指的就是`gl=canvas.getContext('webgl')`返回对象`gl`的一系列绘制渲染方法，通过WebGL API可以把一个三维场景绘制渲染出来。比如上面代码中`gl.createShader()`、`gl.shaderSource()`、`gl.drawArrays()`等方法就是WebGl API。

### 初始化着色器函数`initShader()`

```js
//声明初始化着色器函数
function initShader(gl,vertexShaderSource,fragmentShaderSource){
    //创建顶点着色器对象
    var vertexShader = gl.createShader(gl.VERTEX_SHADER);
    //创建片元着色器对象
    var fragmentShader = gl.createShader(gl.FRAGMENT_SHADER);
    //引入顶点、片元着色器源代码
    gl.shaderSource(vertexShader,vertexShaderSource);
    gl.shaderSource(fragmentShader,fragmentShaderSource);
    //编译顶点、片元着色器
    gl.compileShader(vertexShader);
    gl.compileShader(fragmentShader);

    //创建程序对象program
    var program = gl.createProgram();
    //附着顶点着色器和片元着色器到program
    gl.attachShader(program,vertexShader);
    gl.attachShader(program,fragmentShader);
    //链接program
    gl.linkProgram(program);
    //使用program
    gl.useProgram(program);
    //返回程序program对象
    return program;
}
```

### 绘制方法`gl.drawArrays()`

`gl.drawArrays()`方法的作用就是通知GPU执行着色器代码，然后根据着色器代码在Canvas画布上进行渲染绘制。

### 着色器代码放在script标签中

WebGL着色器代码在JavaScript中以字符串的形式存在，编写代码比较麻烦，为了编写方便，可以把着色器代码写在script或者其他HTML元素标签中，然后通过元素`.innerText`属性获得元素中的字符串，也就是着色器的代码。

在原来WebGL绘制一个点的代码基础上进行更改

```HTML
<!-- 顶点着色器源码 -->
<script id="vertexShader" type="x-shader/x-vertex">
  void main() {
    //给内置变量gl_PointSize赋值像素大小
    gl_PointSize=20.0;
    //顶点位置，位于坐标原点
    gl_Position =vec4(0.0,0.0,0.0,1.0);
  }
</script>
<!-- 片元着色器源码 -->
<script id="fragmentShader" type="x-shader/x-fragment">
  void main() {
    gl_FragColor = vec4(1.0,0.0,0.0,1.0);
  }
</script>
```

```js
//顶点着色器源码
var vertexShaderSource = document.getElementById('vertexShader').innerText;
//片元着色器源码
var fragShaderSource = document.getElementById('fragmentShader').innerText;
//初始化着色器
var program = initShader(gl,vertexShaderSource,fragShaderSource);
```

