## Error对象

一旦代码解析或运行时发生错误，JavaScript引擎就会自动产生并抛出一个Error对象的实例，然后整个程序就中断在发生错误的地方。

Error对象的实例有三个最基本的属性：

- name：错误名称
- message：错误提示信息
- stack：错误的堆栈（非标准属性，但是大多数平台支持）

```js
function throwit() {
  throw new Error('');
}
 
function catchit() {
  try {
    throwit();
  } catch(e) {
    console.log(e.stack); // print stack trace
  }
}
 
catchit()
// Error
//    at throwit (~/examples/throwcatch.js:9:11)
//    at catchit (~/examples/throwcatch.js:3:9)
//    at repl:1:5
```

上面代码显示，抛出错误首先是在throwit函数，然后是在catchit函数，最后是在函数的运行环境中。



Error.prototype 对象包含如下属性：

- constructor--指向实例的构造函数
- message--错误信息
- name--错误的名字(类型)
  上述是 Error.prototype 的标准属性, 此外, 不同的运行环境都有其特定的属性. 在例如 Node, Firefox, Chrome, Edge, IE 10+, Opera 以及 Safari 6+ 这样的环境中, Error 对象具备 stack 属性, 该属性包含了错误的堆栈轨迹. 一个错误实例的堆栈轨迹包含了自构造函数之后的所有堆栈结构.

## 调用栈的工作机制

**函数被调用时，就会被加入到调用栈顶部，执行结束之后，就会从调用栈顶部移除该函数**，这种数据结构的关键在于后进先出，即大家所熟知的 LIFO。比如，当我们在函数 y 内部调用函数 x 的时候，调用栈从下往上的顺序就是 y -> x 。

例子：

```js
function c() {
    console.log('c');
}

function b() {
    console.log('b');
    c();
    console.trace();
}

function a() {
    console.log('a');
    b();
}

a();
```

通过输出结果可以看到，此时打印的调用栈从下往上是：a -> b，已经没有 c 了，因为 c 执行完之后就从调用栈移除了。

```text
Trace
    at b (repl:4:9)
    at a (repl:3:1)
    at repl:1:1  // <-- 从这行往下的内容可以忽略，因为这些都是 Node 内部的东西
    at realRunInThisContextScript (vm.js:22:35)
    at sigintHandlersWrap (vm.js:98:12)
    at ContextifyScript.Script.runInThisContext (vm.js:24:12)
    at REPLServer.defaultEval (repl.js:313:29)
    at bound (domain.js:280:14)
    at REPLServer.runBound [as eval] (domain.js:293:12)
    at REPLServer.onLine (repl.js:513:10)
```

**调用函数的时候，会被推到调用栈的顶部，而执行完毕之后，就会从调用栈移除。**

## Error 对象及错误处理

Error 对象的 prototype 具有以下属性：

- constructor - 负责该实例的原型构造函数；
- message - 错误信息；
- name - 错误的名字；

上面都是标准属性，有些 JS 运行环境还提供了标准属性之外的属性，如 Node.js、Firefox、Chrome、Edge、IE 10、Opera 和 Safari 6+ 中会有 stack 属性，它包含了错误代码的调用栈，接下来我们简称错误堆栈。**错误堆栈包含了产生该错误时完整的调用栈信息**。

抛出错误时，你必须使用 throw 关键字。为了捕获抛出的错误，则必须使用 try catch 语句把可能出错的代码块包起来，catch 的时候可以接收一个参数，该参数就是被抛出的错误。

try 语句：

- try ... catch
- try ... finally
- try ... catch ... finally

```js
try {
    try {
        throw new Error('Nested error.'); // 这里的错误会被自己紧接着的 catch 捕获
    } catch (nestedErr) {
        console.log('Nested catch'); // 这里会运行
    }
} catch (err) {
    console.log('This will not run.');  // 这里不会运行
}
```

