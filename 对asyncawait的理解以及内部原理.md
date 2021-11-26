async / await 是一种更方便地完成异步调用的语法

#### 基本含义

async 使得后面的 function 始终返回一个 promise，无论 function 本身返回的是否是 promise。

await 必须在 async 函数内部使用，只有等到 await 后面的部分执行完成后，函数才会继续往下执行。
举例来说：

```
async function f() {

  const promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000)
  })

  const result = await promise; // wait till the promise resolves (*)

  alert(result)

  alert('end of function')
}

f();
```

执行结果：

```
done
end of function
```

await 会把后面的 promise 放到 microtask queue 中，所以当 await 和 setTimeout 放到一起时，会先执行 await 的部分，再执行 setTimeout 的部分（setTimeout 会进入 macrotask，优先级低于 microtask）。比如：

```
async function f() {
  return 1;
}

(async () => {
    setTimeout(() => alert('setTimeout is done'), 0);

    await f();
    alert('await is done'); 
})();
```

执行结果：

```
await is done
setTimeout is done
```

#### 基本原理

async / await 本质上是 generator 的语法糖，与 generator 相比，多了以下几个特性：

- 内置执行器，无需手动执行 `next()` 方法
- await 后面的函数可以是 promise 对象也可以是普通 function，而 yield 关键字后面必须得是 thunk 函数或 promise 对象

```
async function fn(args) {
  // ...
}
```

等同于：

```
function fn(args) {
  return spawn(function* () {
    // ...
  });
}
```

而 spawn 函数就是所谓的自动执行器了

```
function spawn(genF) {
  return new Promise(function(resolve, reject) {
    const gen = genF();
    function step(nextF) {
      let next;
      try {
        next = nextF();
      } catch(e) {
        return reject(e);
      }
      if(next.done) {
        return resolve(next.value);
      }
      Promise.resolve(next.value).then(function(v) {
        step(function() { return gen.next(v); });
      }, function(e) {
        step(function() { return gen.throw(e); });
      });
    }
    step(function() { return gen.next(undefined); });
  });
}
```

#### 为了能够正确的执行代码，应该对 `await` 进行错误处理，基本的错误处理方式有两类：

```
async function asyncFn6 () {
    try {
        await Promise.reject('error')
    } catch (err) {
        console.log(err)
    }
    return await Promise.resolve('hello async')
}
// 将可能发生错误的函数使用try...catch进行处理

async function asyncFn7 () {
    await Promise.reject('error').catch(err => console.log(err))
    return await Promise.resolve('hello async')
}
// 将可能变为reject状态的Promise对象后面跟上一个catch方法，以处理之前发生的错误
```


作者：馒头君
链接：https://juejin.cn/post/6844903773911908359
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# [async/await 优雅的错误处理方法](https://juejin.cn/post/6844903767129718791)

```js
// 抽离成公共方法
    const awaitWrap = (promise) => {
        return promise
            .then(data => [null, data])
            .catch(err => [err, null])
    }
```

```tsx
function awaitWrap<T, U = any>(promise: Promise<T>): Promise<[U | null, T | null]> {
    return promise
        .then<[null, T]>((data: T) => [null, data])
        .catch<[U, null]>(err => [err, null])
}
```

#### 使用



