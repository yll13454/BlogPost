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

## Promise.any()

ES2021 引入了Promise.any()方法。该方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例返回。只要参数实例有一个变成fulfilled状态，包装实例就会变成fulfilled状态；如果所有参数实例都变成rejected状态，包装实例就会变成rejected状态。

`Promise.any()`跟`Promise.race()`方法很像，只有一点不同，就是不会因为某个 Promise 变成`rejected`状态而结束。

```js
var resolved = Promise.resolve(42);
var rejected = Promise.reject(-1);
var alsoRejected = Promise.reject(Infinity);

Promise.any([resolved, rejected, alsoRejected]).then(function (result) {
  console.log(result); // 42
});

Promise.any([rejected, alsoRejected]).catch(function (results) {
  console.log(results); // [-1, Infinity]
});
```

## Promise之微任务宏任务

setTimeout 和 Promise 并不在一个异步队列中，前者属于宏任务（MacroTask），而后者属于微任务（MicroTask）

```js
setTimeout(_ => console.log(4))//异步宏任务
new Promise(resolve => {
  console.log(1)//Promise在实例化的过程中所执行的代码都是同步进行的
  resolve()
}).then(_ => {
  console.log(3)//then则是具有代表性的微任务
})

console.log(2)//同步
```



```js
// 这是一个同步任务
console.log('1')            --------> 直接被执行
                                      目前打印结果为：1

// 这是一个宏任务
setTimeout(function () {    --------> 整体的setTimeout被放进宏任务列表
  console.log('4')                    目前宏任务列表记为【c4】
});

new Promise(function (resolve) {
  // 这里是同步任务
  console.log('2');         --------> 直接被执行
  resolve();                          目前打印结果：1，2
  // then是一个微任务
}).then(function () {       --------> 整体的then[包含里面的setTimeout]被放进微任务列表
  console.log('3')                    目前微任务列表记为【c3】，目前打印结果 1，2，3
  setTimeout(function () {
    console.log('5')        -------->微任务里面如果有宏任务，那么会将宏任务放进任务列表
                                      目前宏任务列表记为【c4，c5】
  });
});
```

## async函数

`async` 函数是什么？

1. async/await是写异步代码的新方式，以前的方法有回调函数和Promise。
2. async/await是基于Promise实现的，它不能用于普通的回调函数。
3. async/await使得异步代码看起来像同步代码，这正是它的魔力所在。
4. async/await解决了Promise可能出现的嵌套地狱。

**async用于申明function异步，await用于等待一个异步方法执行完成**

**1、正常情况下，await命令后面是一个Promise对象。如果不是，会被转成一个立即resolve的Promise对象。**

**2、await命令后面的 Promise 对象如果变为reject状态，则reject的参数会被.catch方法的回调函数接收到**

```js
//下面代码中，await语句前面没有return，但是reject方法的参数依然传入了catch方法的回调函数。这里如果在await前面加上return，效果是一样的。
async function f() {
  await Promise.reject('出错了');
}
f()
.then(v => 
    console.log(v)
)
.catch(e => 
    console.log(e)
)  //出错了
```

**3、Promise 对象的状态变化：async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。**

**只要一个await语句后面的 Promise 变为reject，那么整个async函数都会中断执行**

```js
async function f() {  
    await Promise.reject('出错了');
    await Promise.resolve('hello world');   //不会执行}  
     //Promise {<rejected>: "出错了"}
```

**4、前一个异步操作失败，也不要中断后面的异步操作。这时可以将第一个await放在try...catch结构里面，这样不管这个异步操作是否成功，第二个await都会执行**

```js
 async function f() {
  try {
      await Promise.reject('出错了');
  } catch(e) {
  }
  return await Promise.resolve('hello world');
}
f()
.then(v => 
    console.log(v)
)  //hello world
 
 
//另一种方法：await后面的 Promise 对象再跟一个catch方法，处理前面可能出现的错误
async function f() {
    await Promise.reject('出错了')
          .catch(e => 
              console.log(e)
          );  //出错了
    return await Promise.resolve('hello world');
}
f()
.then(v => 
    console.log(v)
)  //hello world
```

**5、如果await后面的异步操作出错，那么等同于async函数返回的 Promise 对象被reject**

```js
async function f() {
    await new Promise(function (resolve, reject) {
        throw new Error('出错了');
    });
}
f()
.then(v => 
    console.log(v)
)
.catch(e => 
    console.log(e)
)  //Error：出错了
```

**6、多个await命令后面的异步操作，如果不存在继发关系，最好让它们同时触发**

```tsx
let foo = await getFoo();
let bar = await getBar();
//上面代码中，getFoo和getBar是两个独立的异步操作（即互不依赖），被写成继发关系。这样比较耗时，因为只有getFoo完成以后，才会执行getBar，完全可以让它们同时触发改进
let [foo, bar] = await Promise.all([getFoo(), getBar()]);
//上面写法，getFoo和getBar都是同时触发，这样就会缩短程序的执行时间 
```

当try...catch包裹在promise对象中的时候，使用.catch可以捕获到所有错误，当try...catch包裹在Promise对象外的时候，catch方法能够捕获全部，而.catch不能能捕获到reject之外的异常。

> 其实说白了就是try/catch捕获异步方法的话，它只会捕获第一次异常，其它的异步所抛出的异常它就捕获不到了。最好的办法就是在async/await中用.catch去捕获对应异步函数reject抛出的错误，然后用try/catch包裹整个代码执行逻辑，这样的话try/catch就可以捕获所有的异常了

```js
const fn = (type, msg) => {
    return new Promise((resolve, reject) => {
        if (type) {
            resolve(`success:${msg}`)
        } else {
            reject(`fail:${msg}`)
        }
    })
}
// 捕获异步中的错误1
const asyncFn = async () => {
    try {
        let result1 = await fn(false, 'hello')
        console.log('中间内容输出')
        let result2 = await fn(false, 'world')
        console.log('result1' + result1)
        console.log('result2' + result2)
    } catch (error) {
        console.log('catch:' + error)
    }
}
// 捕获异步中的错误1
//catch:fail:hello

const asyncFn = async () => {
    try {
        await fn(false, 'hello').then(() => {}).catch(err => {
            console.log('result1:' + err)
        })
        console.log('中间内容输出')
        await fn(false, 'world').then(() => { }).catch(err => {
            console.log('result2:' + err)
        })
    } catch (error) {
        console.log('catch:' + error)
    }
}

asyncFn();
// 捕获异步中的错误2
//result1:fail:hello
//中间内容输出
//result2:fail:world
```

### **try catch中如果有多个await时，多个错误，catch只能补获第一个错误。**

