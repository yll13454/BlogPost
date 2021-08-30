结论:

1. try catch不能捕获异步代码,所以不能捕获promise.reject()的错误,并且promise期约故意将异步行为封装起来，从而隔离外部的同步代码
2. try catch 能对promise的reject()落定状态的结果进行捕获
3. try catch能捕捉到的异常，必须是`主线程执行`已经进入 try catch, 但 try catch 尚未执行完的时候抛出来的,
意思是如果将执行try catch分为前中后.只有中才能捕获到异常
4. 应该只在确切知道接下来该做什么的时候捕获错误(这里我单指try catch)

### try catch基本介绍

```js
try {
// 可能出错的代码
} catch (e) {
// 出错时要做什么
}
```

#### try 或 catch 块无法阻止 finally 块执行

3.2.1调用了window没有的方法, 必然报错,走catch,这时候finanlly有返回

```javascript
   try {
      window.someNonexistentFunction();
      console.log('try')
    } catch(e) {
      console.log(e, 'catch')
    } finally {
      console.log('finally')
    }
```


![img](media/9d34fe3c0073435dad7205c0b3e1e57atplv-k3u1fbpfcp-watermark.awebp)

3.2.2没有报错,这时候finanlly有返回

```javascript
    try {
      console.log('try')
    } catch(e) {
      console.log(e, 'catch')
    } finally {
      console.log('finally')
    }
```

3.2.3那么对于finally我们能用他做啥呢?

```
1.例如loading的取消,不管我报错没, 最后这个loading必须是要取消的
2.比如做一些清除,销毁的操作
```

#### 3.3 return 语句无法阻止finally的执行

### 四 try catch是如何捕获错误的?什么时候才能捕获到错误 (`非常重要`)

try catch能捕捉到的异常，必须是`主线程执行`已经进入 try catch, 但 try catch 尚未执行完的时候抛出来的,意思是如果将执行try catch分为前中后.只有中才能捕获到异常

#### 4.1 try catch 之前 (`捕获不到的`)

![img](media/3f86539587874e25b218d50ab992b6c9tplv-k3u1fbpfcp-watermark.awebp)

![img](media/5cc5cf1d3aa1465c8505e15e14f88c99tplv-k3u1fbpfcp-watermark.awebp)

发现了没? 连6666都没有打印,比如语法异常（syntaxError），因为语法异常是在语法检查阶段就报错了，线程执行尚未进入 try catch 代码块，自然就无法捕获到异常

#### 4.2 try catch进行之中(`能捕获到`)

```javascript
    try {
      window.someNonexistentFunction();
    } catch(e){
       console.log("error",e);
    }
    console.log('我要执行')
```

#### 4.3 try catch进行之中(`重点:不能捕获到`)

```javascript
 try {
      console.log('try里面')
      setTimeout(() => {
        console.log('try里面的setTimeOut')
        window.someNonexistentFunction(); // 变成了未捕获的错误
      }, 0)
    } catch (e) {
      console.log('error', e)
    }
    setTimeout(() => {
      console.log('我要执行')
    }, 100)
复制代码
```

![img](media/b0c803d77dcf4342915370a3b72117d9tplv-k3u1fbpfcp-watermark.awebp)


`这里没有catch到错误,是因为try catch是同步代码`(一般我们用setTimeOut来模拟异步,也是因为他有延迟的特性,所以我们这里视为有异步操作)

```
当js运行到try里面setTimeOut的时候,setTimeOut先是去了setTimeOut线程,0秒后去task queue(任务队列)进行排队,运行到try catch外面的setTimeOut, 第二个setTimeOut也是先去setTimeOut线程,100/1000秒然后去task queue排队,排在第一个后面

那么执行的顺序就是try catch同步代码在主执行栈执行完了之后,去task queue看看有啥要执行的没,task queue里面是遵循先进先出的原则,自然按照图片上的顺序进行
```

这个例子也是说明了为啥try catch不能捕获到promise.resolve报错的一个原因

#### 4.4 Promise的异常捕获

**如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。**

捕获Promise内部错误的两种方式以及reject回调后的错误捕获的一种方式

​		**第一种**

 1. Promise.prototype.then() reject回调函数

 2. Promise.prototype.catch()

    **第二种**


 1. 结合async/await和try...catch

##### 4.4.1 Promise.prototype.then()捕获-- reject回调函数

特点:

```
1.无法捕获resolve()回调中出现的异常，需要无限链式调用then回调去捕获异常
2.无法中断后续的then操作
```

- 能够在第二个reject回调中捕获到,如果又有报错,能在下一个then里面的reject回调捕获到

- 如果已经是resove的回调了,中间有报错,也能够在下一个then里面的reject回调能捕获到

- 能够在reject的回调函数里面去使用try catch,因为这里面就是同步代码,但是不用直接去捕获 Promise.reject()

##### 4.4.2 Promise.prototype.catch()捕获异常

特点:

因为Promise 对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获。
      catch()方法返回的还是一个 Promise 对象，因此后面还可以接着调用then()方法。

**记住一点: `Promise.prototype.catch()` 方法用于给期约添加拒绝处理程序, 这个方法只接收一个参数：onRejected 处理程序。事实上，这个方法就是一个语法糖，调用它就相当于调用 `Promise.prototype.then(null, onRejected)`**

- 冒泡性质的抛错

```js
   const createPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        reject('promise')
      }, 1000)
    })
    createPromise.then((res) => {
      console.log(res, 'resolved')
    }).then(() => {
      console.log(res, 'resolved')
    }).then(() => {
      console.log(res, 'resolved')
    }).then(() => {
      console.log(res, 'resolved')
    }, res => {
      console.log(res, 'reject')
    })
```

- catch也同reject回调一致,具有捕获错误的冒泡性质,只要有报错,不管经过创建多少个新的promise,后面仍然能捕获到

```js
   const createPromise = new Promise((resolve, reject) => {
      setTimeout(() => {
        reject('promise')
      }, 1000)
    })
    createPromise.then((res) => {
      console.log(res, 'resolved')
    }).then(() => {
      console.log(res, 'resolved')
    }).then(() => {
      console.log(res, 'resolved')
    }).catch((res) => {
      console.log(res, 'catch reject');
    })
```

-  catch 到了错误,又是一个新的promise,cath回调里面没有报错,下一个then还是走resolved的回调

```js
  const createPromise = new Promise((resolve, reject) => {
       reject('promise');
    })
    createPromise.then(res => {
      console.log('resolved1', res)
    })
    .then(res => {
      console.log('resolved2', res)
    })
    .catch(res=> {
      console.log('catche reject', res)
    })
    .then(res => {
      console.log('resolved3', res)
    })
```

![img](media/b880c3bb2e91481fb69b54d3d5e2c692tplv-k3u1fbpfcp-watermark.awebp)

##### 4.4.3 async/ await结合try...catch使用

什么是async函数?

```
1. async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await命令就是内部then命令的语法糖。
2. async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，
等到异步操作完成，再接着执行函数体内后面的语句。
```

什么是await命令？有那些特点

```
1. await只能在异步函数async function中使用。
2. await表达式会暂停当前async function的执行，等待Promise处理完成。若Promise正常处理(fulfilled)，
其回调的resolve函数参数作为await表达式的值，继续执行async function。
3. 若Promise处理异常(rejected)，await表达式会把Promise的异常原因抛出。
4. await如果返回的是reject状态的promise，如果不被捕获抛出，就会中断async函数的执行。
5. 另外，如果await操作符后的表达式的值不是一个Promise，则返回该值本身。
6.async await函数中，通常使用try/catch块来捕获错误。
```

**await之后的返回值就是 resolve和reject回调的结果**

调用一个请求,失败了,就要中断我们所有的代码执行,显然不是我们想要的结果,那么如何让调用接口失败,我们仍然可以继续操作呢?

```js
  const createPromise = new Promise((resove, reject) => {
      setTimeout(() => {
        console.log('我反正进来了')
        reject('promise')
      }, 1000)
    })
    async function awaitTest() {
      try {
        const res = await createPromise
      } catch(e) {
        console.log(e, 'await调用报错咯!');
      } 
      console.log('可以让我执行一下吗?');
    }
    awaitTest()
```

![img](media/a15aecc692424e71b7a5f279760ee0e9tplv-k3u1fbpfcp-watermark.awebp)

当promise函数里面调用reject()来落定拒绝契约的时候,实际上

```
 reject(XXXX) 相当于 throw new Error(xxxx)
```

![img](media/c3be7fce271b42938a67d0c078b6ad2ctplv-k3u1fbpfcp-watermark.awebp)

##### 不是落定状态,我能catch到错误吗?

```js
 const createPromise = new Promise((resove, reject) => {
      setTimeout(() => {
        throw new Error('我是个错误,但是我并不想落定状态,没有reject()')
      }, 1000)
    })

    createPromise.then((res) => {
      console.log(res, 'resolved');
    }).catch(res => {
      console.log(res, 'catch');
    })
```

因为then只有两种状态,resolve或者reject的结果,所以在promise里面没有调用resolve()或者reject(),根本反应不到内部

##### await都拿不到结果,那就更加别提能用try catch能捕获了, await表示的是落定状态(fulfilled)返回的结果

```js
   const createPromise = new Promise((resove, reject) => {
      setTimeout(() => {
        throw new Error('我是个错误,但是我并不想落定状态,没有reject()')
      }, 1000)
    })
    async function awaitTest() {
      try {
        console.log('测试1')
        const res = await createPromise // 调用的时候内部已经中止了,也不是settled的状态,所以没法去返回结果,那就更不可能进行到赋值给res的操作了
        console.log(res, '测试2')
      } catch(e) {
        console.log(e, 'await调用报错咯!');
      } 
      console.log('可以让我执行一下吗?');
    }
    awaitTest()
```

![img](media/12519da00f1c448eb33988b16c0d3eadtplv-k3u1fbpfcp-watermark.awebp)

##### try catch 面对多个await捕获的问题

```js
const createPromise1 = new Promise((resolve, reject) => {
      setTimeout(() => {
        reject('失败1')
      }, 1000)
    })
    const createPromise2 = new Promise((resolve, reject) => {
      setTimeout(() => {
        reject('失败2')
      }, 1000)
    })
    const createPromise3 = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('成功3')
      }, 1000)
    })
    async function awaitTest() {
      try {
        await createPromise1
        console.log('不执行');
        await createPromise2
        await createPromise3
      } catch(e) {
        console.log(e, 'await调用报错咯!');
      } 
      console.log('1可以让我执行一下吗?');
    }
    awaitTest()
    console.log('2可以让我执行一下吗?');
```

结果就是多个await 有几个报错,她只会catch到第一个,走了catch,但是仍然可以继续运行

#### 4.5 try catch不能捕获到promise.resolve()的报错

4.5.1 try catch捕获throw new Error 这个没有问题

```javascript
     try { 
        throw new Error('foo'); 
      } catch(e) { 
        console.log(e, 'catch'); 
      }
```

![img](media/48531a7f436b41ffb6eeb45a65b86013tplv-k3u1fbpfcp-watermark.awebp)

4.5.2 try catch 捕获不到 Promise.reject()

```javascript
   try { 
      Promise.reject()
    } catch(e) { 
      console.log(e, 'catch'); 
    }
```

![img](media/4b5a0f1ed26443b5b4b6cb923a21de4etplv-k3u1fbpfcp-watermark.awebp)

下面两个契约实例其实是一样的

```javascript
let p1 = new Promise((resolve, reject) => reject());
let p2 = Promise.reject();
```

p1,p2是异步执行模式,而try catch是同步代码,同步代码先执行,异步去task queue排队,等同步做完再执行,而你能够在then的回调里面进行try catch或者await的返回数,只是对返回结果进行捕获而已,并捕获不了promise的异步代码

![img](media/6412fa184a054ea99ca6495dc5888562tplv-k3u1fbpfcp-watermark.awebp)

### 五 try catch啥时候用,以及如何抛错

##### 5.1 throw new Error()

 使用 throw 操作符时，代码立即停止执行，除非 try/catch 语句捕获了抛出的值

```js
   try { 
        window.someNonexistentFunction()
      } catch(e) { 
        try {
          throw new Error('报错了')
        } catch(e) {
          console.log('我要执行')
        } 
      }
```

#### 5.2 应该仔细评估每个函数，以及可能导致它们失败的情形。良好的错误处理协议可以保证只会发生你自己抛出的错误。

![img](media/bd576894bf314e0f96a30ccbc6772dbatplv-k3u1fbpfcp-watermark.awebp)

#### 5.3 通过继承自定义错误类型,与提示

```js
class CustomError extends Error {
    constructor(message) {
        super(message);
        this.name = "CustomError";
        this.message = message;
    }
}
throw new CustomError("My message");
```

### 六 有哪些错误类型

```
1 Error 
2 InternalError
3 EvalError
4 RangeError
5 ReferenceError
6 SyntaxError
7 TypeError
8 URIError
```

1.Error 是基类型，其他错误类型继承该类型。

2.InternalError 该错误在JS引擎内部发生，特别是当它有太多数据要处理并且堆栈增长超过其关键限制时。示例场景通常为某些成分过大

```
该错误在JS引擎内部发生，特别是当它有太多数据要处理并且堆栈增长超过其关键限制时。
示例场景通常为某些成分过大，例如：
“too many switch cases”（过多case子句）；
“too many parentheses in regular expression”（正则表达式中括号过多）；
“array initializer too large”（数组初始化器过大）；
“too much recursion”（递归过深）。
```

3.EvalError 类型的错误会在使用 eval() 函数发生异常时抛出,如果非法调用 eval()，则抛出 EvalError 异常

4.RangeError 错误会在数值越界时抛出

```js
new Array(-20)  // 异常的捕获.html:52 Uncaught RangeError: Invalid array length
const num = 1
let res = num.toFixed(-1)
console.log(res); // 异常的捕获.html:53 Uncaught RangeError: toFixed() digits argument must be between 0 and 100
```

5.ReferenceError 会在找不到对象时发生

6.TypeError 在 JavaScript 中很常见，主要发生在变量不是预期类型，或者访问不存在的方法时

7.URIError，只会在使用 encodeURI()或 decodeURI()但传入了格式错误的URI 时发生

###  调试技术

#### 9.1 debugger 关键字

#### 9.2 在页面中打印消息

#### 9.3 根据需求封装console.log()

### 抛出错误

