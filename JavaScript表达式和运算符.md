# 加法赋值 (+=)

加法赋值操作符 (`+=`) 将右操作数的值添加到变量，并将结果分配给该变量。两个操作数的类型确定加法赋值运算符的行为。加法或串联是可能的。

```js
let a = 2;
let b = 'hello';

console.log(a += 3); // addition
// expected output: 5

console.log(b += ' world'); // concatenation
// expected output: "hello world"
```

>```
>Operator: x += y
>Meaning:  x  = x + y
>```

# async function expression

**`async function`** 关键字用来在表达式中定义异步函数。当然，你也可以用 [`异步函数语句`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 来定义。

> async function [name]([param1[, param2[, ..., paramN]]]) { statements }

# await

`await` 操作符用于等待一个[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象。它只能在异步函数 [`async function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/async_function) 中使用。

**语法**

```
[返回值] = await 表达式;
```

- 表达式

  一个 [`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象或者任何要等待的值。

- 返回值

  返回 Promise 对象的处理结果。如果等待的不是 Promise 对象，则返回该值本身。

```js
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async function f1() {
  var x = await resolveAfter2Seconds(10);
  console.log(x); // 10
}
f1();
```

如果一个 Promise 被传递给一个 await 操作符，await 将等待 Promise 正常处理完成并返回其处理结果。

如果该值不是一个 Promise，await 会把该值转换为已正常处理的Promise，然后等待其处理结果。

# AND(运算符）

AND在编程术语中表示一种运算方法，不可逆。

```
常用符号：&（按位与），&&（逻辑与）
其运算规则如下：
1&1=1; true&&true=true;
1&0=0; true&&false=false;
0&1=0; false&&true=false;
0&0=0; false&&false=false;
即与0则0，常用此特性来将某些位置0或保存某些位。
与运算,二进制运算．可逆运算．1 and 1=1,1 and 0=0,0 and 0=0，0 and 1=0.
a and b 的运算方法：将a和b转换成2进制后,一位一位地去比较,当两个位都是1时,那么结果为1,否则为0.最后再把它转换成十进制就可以了.
```

# 按位与赋值(&=)

按位与赋值运算符（＆=）表示两个操作数的二进制，对它们进行按位AND运算并将结果分配给变量。

```
let a = 5;
// 5:     00000000000000000000000000000101
// 2:     00000000000000000000000000000010
a &= 2; // 0
```

# 按位与 (&)

按位与运算符 (`&`) 在每个位上返回 `1` ，这两个操作数对应的位都是 `1`.

```
const a = 5;        // 00000000000000000000000000000101
const b = 3;        // 00000000000000000000000000000011

console.log(a & b); // 00000000000000000000000000000001
// expected output: 1
```

# 按位非 (~)

按位非运算符（~），反转操作数的位。

```
const a = 5;     // 0000000000000101
const b = -3;    // 1111111111111101

console.log(~a); // 1111111111111010
// expected output: -6

console.log(~b); // 0000000000000010
// expected output: 2
```

# 按位或赋值（|=）

按位或赋值运算符（|=）使用两个操作数的二进制表示，对它们执行按位或运算，并将结果赋给变量。

```
let a = 5;      // 00000000000000000000000000000101
a |= 3;         // 00000000000000000000000000000011

console.log(a); // 00000000000000000000000000000111
// expected output: 7
```

# 按位或（|）

按位或运算符（|）在每个位位置返回1，其中一个或两个操作数的对应位为1。

```
const a = 5;        // 00000000000000000000000000000101
const b = 3;        // 00000000000000000000000000000011

console.log(a | b); // 00000000000000000000000000000111
// expected output: 7
```

# 按位异或赋值 (^=)

按位异或赋值操作符 (`^=`) 使用二进制表示操作数，进行一次按位异或操作并赋值。

```
let a = 5;      // 00000000000000000000000000000101
a ^= 3;         // 00000000000000000000000000000011

console.log(a); // 00000000000000000000000000000110
// expected output: 6
```

# 位异或（^）

按位异或运算符（^）在每个位位置返回1，其中一个操作数（而不是两个操作数）的对应位为1。

```
const a = 5;        // 00000000000000000000000000000101
const b = 3;        // 00000000000000000000000000000011

console.log(a ^ b); // 00000000000000000000000000000110
// expected output: 6
```

# 类表达式

**类表达式**是用来定义类的一种语法。和[函数表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/function)相同的一点是，类表达式可以是命名也可以是匿名的。如果是命名类表达式，这个名字只能在类体内部才能访问到。JavaScript 的类也是基于原型继承的。

```
const MyClass = class [className] [extends] {
  // class body
};
```

# 逗号操作符

**逗号操作符** 对它的每个操作数求值（从左到右），并返回最后一个操作数的值。

```js
let x = 1;

x = (x++, x);

console.log(x);
// expected output: 2

x = (2, 3);

console.log(x);
// expected output: 3
```

# 条件运算符

**条件（三元）运算符**是 JavaScript 仅有的使用三个操作数的运算符。一个条件后面会跟一个问号（?），如果条件为 [truthy](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) ，则问号后面的表达式A将会执行；表达式A后面跟着一个冒号（:），如果条件为 [falsy](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy) ，则冒号后面的表达式B将会执行。本运算符经常作为 `if` 语句的简捷形式来使用。

```js
function getFee(isMember) {
  return (isMember ? '$2.00' : '$10.00');
}

console.log(getFee(true));
// expected output: "$2.00"

console.log(getFee(false));
// expected output: "$10.00"

console.log(getFee(null));
// expected output: "$10.00"
```

# 自减 (--)

 自减运算符(`--`) 将它的操作数减一，然后返回操作数.

```js
let x = 3;
const y = x--;

console.log(`x:${x}, y:${y}`);
// expected output: "x:2, y:3"

let a = 3;
const b = --a;

console.log(`a:${a}, b:${b}`);
// expected output: "a:2, b:2"
```

# delete 操作符

 **`delete` 操作符**用于删除对象的某个属性；如果没有指向这个属性的引用，那它最终会被释放。

```js
const Employee = {
  firstname: 'John',
  lastname: 'Doe'
};

console.log(Employee.firstname);
// expected output: "John"

delete Employee.firstname;

console.log(Employee.firstname);
// expected output: undefined
```

> delete object.property
> delete object['property']

# 解构赋值

**解构赋值**语法是一种 Javascript 表达式。通过**解构赋值,** 可以将属性/值从对象/数组中取出,赋值给其他变量。

# 除法赋值（/=）

除法赋值运算符（/=）将变量除以右操作数的值，并将结果赋给变量。

```js
let a = 3;

console.log(a /= 2);
// expected output: 1.5

console.log(a /= 0);
// expected output: Infinity

console.log(a /= 'hello');
// expected output: NaN
```

# 指数赋值（**=）

求幂赋值运算符（**=）将变量的值提升到右操作数的幂。

```js
let a = 3;

console.log(a **= 2);
// expected output: 9

console.log(a **= 0);
// expected output: 1

console.log(a **= 'hello');
// expected output: NaN
```

# function* 表达式

**`function\*`**关键字可以在表达式内部定义一个生成器函数。

```js
function* foo() {
  yield 'a';
  yield 'b';
  yield 'c';
}

let str = '';
for (const val of foo()) {
  str = str + val;
}

console.log(str);
// expected output: "abc"
```

