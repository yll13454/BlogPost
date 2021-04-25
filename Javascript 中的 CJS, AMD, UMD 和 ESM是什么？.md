## 模块化

**`CJS`、`AMD`、`UMD` 和 `ESM`**

### CLS

`CJS` 是 `CommonJS` 的缩写。

```js
// importing 
const doSomething = require('./doSomething.js'); 

// exporting
module.exports = function doSomething(n) {
  // do something
}
```

- 通过`require`函数进行导入
- 通过`export`或者`module.exports`导出
- 同步加载

- 当 `CJS` 导入时，它会给你一个导入对象的副本
- `CJS` 不能在浏览器中工作。它必须经过转换和打包

### AMD

`AMD` 代表异步模块定义。

[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)`Async Module Definition`代表的意思为异步模块定义，是`Javascript`模块化的浏览器解决方案，它采用异步的方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在回调函数中，等到加载完成之后，这个回调函数才会运行。

[AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)规范定义了一个函数`define`，通过`define`方法定义模块：

```
 define(id?, dependencies?, factory);
```

并且采用`require()`语句加载模块：

```
require([module], callback);
```

引入的模块和回调函数不是同步的，所以浏览器不会因为引入的模块加载不成功而假死。

```javascript
define(['dep1', 'dep2'], function (dep1, dep2) {
    //Define the module value by returning a value.
    return function () {};
});
复制代码
```

或者

```javascript
// "simplified CommonJS wrapping" https://requirejs.org/docs/whyamd.html
define(function (require) {
    var dep1 = require('dep1'),
        dep2 = require('dep2');
    return function () {};
});
```

- `AMD` 是异步(`asynchronously`)导入模块的(因此得名)
- 一开始被提议的时候，`AMD` 是为前端而做的(而 `CJS` 是后端)

### UMD

全称 Universal Module Definition（万能模块定义），从名字就可以看出 UMD 做的是大一统的工作。

这个万能模块，可以在服务端使用，也可以在浏览器端使用；

它主要做了三件事：

- 判断是否支持AMD
- 判断是否支持CommonJS
- 如果都不支持，使用全局变量

```js
(function (root, factory) {
    if (typeof define === "function" && define.amd) {
        define(["jquery", "underscore"], factory);
    } else if (typeof exports === "object") {
        module.exports = factory(require("jquery"), require("underscore"));
    } else {
        root.Requester = factory(root.$, root._);
    }
}(this, function ($, _) {
    // this is where I defined my module implementation

    var Requester = { // ... };

    return Requester;
}));
```

- 在前端和后端都适用（“通用”因此得名）
- 与 `CJS` 或 `AMD` `不同，UMD` 更像是一种配置多个模块系统的模式。[这里](https://github.com/umdjs/umd/)可以找到更多的模式
- 当使用 `Rollup/Webpack` 之类的打包器时，`UMD` 通常用作备用模块

### ESM

全称 ECMAScript Module

- 使用 import 导入模块；
- 使用 export 导出模块；



- 在很多[现代浏览器](https://caniuse.com/es6-module)可以使用
- 它兼具两方面的优点：具有 `CJS` 的简单语法和 `AMD` 的异步
- 得益于 `ES6` 的[静态模块结构](https://exploringjs.com/es6/ch_modules.html#sec_design-goals-es6-modules)，可以进行 [ Tree Shaking](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking/)
- `ESM` 允许像 `Rollup` 这样的打包器，[删除不必要的代码](https://dev.to/bennypowers/you-should-be-using-esm-kn3)，减少代码包可以获得更快的加载
- 可以在 `HTML` 中调用，只要如下

```javascript
<script type="module">
  import {func1} from 'my-lib';

  func1();
</script>
复制代码
```

但是不是所有的浏览器都支持（[来源](https://jakearchibald.com/2017/es-modules-in-browsers/)）

## 总结

- 由于 `ESM` 具有简单的语法，异步特性和可摇树性，因此它是最好的模块化方案
- `UMD` 随处可见，通常在 `ESM` 不起作用的情况下用作备用
- `CJS` 是同步的，适合后端
- `AMD` 是异步的，适合前端