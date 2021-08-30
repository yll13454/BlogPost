## 迭代器是什么

>遍历和迭代有什么区别？

**遍历**：

对于这些数据类型的不同，我们可能需要不同的遍历方式，比如对象我们需要通过 `for...in`，函数也不一样。

**迭代**：

对于不同数据类型可以用同一种遍历方式

>迭代就是从目标源依次按逐个抽取的方式来提取数据，其中目标源满足：1、有序的 2、连续的 
>
>而遍历就没有这些要求，对于不同数据类型会有不同遍历方式。

### 迭代引入

可以直接迭代出来的数据类型

>Array、Map、Set、String、TypeArray、arguments、NodeList





如果你用正常的函数让两个数字，你会立即得到结果.。但是，当你发出远程调用来得到结果时，你需要等待，你不能立即得到结果.。

或者这样说，你不知道你是否会得到结果，因为服务器可能会下降，响应速度慢等，你不希望在等待结果的时候，整个进程被阻止。调用API，下载文件，读取文件中的一些你要执行的常用的异步操作。

## Promise 的含义

`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。

### 特点

（1）对象的状态不受外界影响。

`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。

（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。

**如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。**这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### `Promise`也有一些缺点。

首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。

**其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。**

第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

## Promise.resolve()

### 参数分成四种

**（1）参数是一个 Promise 实例**

如果参数是 Promise 实例，那么`Promise.resolve`将不做任何修改、原封不动地返回这个实例。

**（2）参数是一个`thenable`对象**

`thenable`对象指的是具有`then`方法的对象，比如下面这个对象。

```js
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
```

`Promise.resolve`方法会将这个对象转为 Promise 对象，然后就立即执行`thenable`对象的`then`方法。

```js
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};
let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});
```

上面代码中，`thenable`对象的`then`方法执行后，对象`p1`的状态就变为`resolved`，从而立即执行最后那个`then`方法指定的回调函数，输出 42。

**（3）参数不是具有`then`方法的对象，或根本就不是对象**

如果参数是一个原始值，或者是一个不具有`then`方法的对象，则`Promise.resolve`方法返回一个新的 Promise 对象，状态为`resolved`。

**（4）不带有任何参数**

`Promise.resolve()`方法允许调用时不带参数，直接返回一个`resolved`状态的 Promise 对象。

所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用`Promise.resolve()`方法。

**注意的是，立即`resolve()`的 Promise 对象，是在本轮“事件循环”（event loop）的结束时执行，而不是在下一轮“事件循环”的开始时。**

```
setTimeout(function () {
  console.log('three');
}, 0);
Promise.resolve().then(function () {
  console.log('two');
});
console.log('one');
// one
// two
// three
```

上面代码中，`setTimeout(fn, 0)`在下一轮“事件循环”开始时执行，`Promise.resolve()`在本轮“事件循环”结束时执行，`console.log('one')`则是立即执行，因此最先输出。

## Promise.reject()

`Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected`。

**注意，`Promise.reject()`方法的参数，会原封不动地作为`reject`的理由，变成后续方法的参数。这一点与`Promise.resolve`方法不一致。**

```js
const thenable = {
  then(resolve, reject) {
    reject('出错了');
  }
};
Promise.reject(thenable)
.catch(e => {
  console.log(e === thenable)
})
// true
```

`Promise.reject`方法的参数是一个`thenable`对象，执行以后，后面`catch`方法的参数不是`reject`抛出的“出错了”这个字符串，而是`thenable`对象。

## 应用

### 加载图片

我们可以将图片的加载写成一个`Promise`，一旦加载完成，`Promise`的状态就发生变化。

```
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    const image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```

## Promise.try()

```
Promise.resolve().then(f)
```

上面的写法有一个缺点，就是如果`f`是同步函数，那么它会在本轮事件循环的末尾执行。

```
const f = () => console.log('now');
Promise.resolve().then(f);
console.log('next');
// next
// now
```

上面代码中，函数`f`是同步的，但是用 Promise 包装了以后，就变成异步执行了。

上面代码中，第二行是一个立即执行的匿名函数，会立即执行里面的`async`函数，因此如果`f`是同步的，就会得到同步的结果；如果`f`是异步的，就可以用`then`指定下一步，就像下面的写法。

```js
(async ()=> f())()
.then(...)
```

需要注意的是，`async () => f()`会吃掉`f()`抛出的错误。所以，如果想捕获错误，要使用`promise.catch`方法。

```js
(async ()=> f())()
.then(...)
.catch(...)
```

第二种写法是使用`new Promise()`。

```js
const f =()=> console.log('now');
(
()=>newPromise(
    resolve => resolve(f())
)
)();
console.log('next');
// now
// next
```

上面代码也是使用立即执行的匿名函数，执行`new Promise()`。这种情况下，同步函数也是同步执行的。

**统一用`promise.catch()`捕获所有同步和异步的错误。**

```
Promise.try(()=> database.users.get({id: userId})).then(...).catch(...)
```

事实上，`Promise.try`就是模拟`try`代码块，就像`promise.catch`模拟的是`catch`代码块。

异步加载图片的例子。

```js
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    const image = new Image();
    image.onload = function() {
      resolve(image);
    };
    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };
    image.src = url;
  });
}
```

