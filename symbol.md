>**symbol** 是一种基本数据类型 （[primitive data type](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)）。

`Symbol()`函数会返回**symbol**类型的值，该类型具有静态属性和静态方法。

**不支持语法："`new Symbol()`"。**

每个从`Symbol()`返回的symbol值都是唯一的。

```js
var sym1 = Symbol();
var sym2 = Symbol('foo');
var sym3 = Symbol('foo');
```

注意，`Symbol("foo")` 不会强制将字符串 “foo” 转换成symbol类型。它每次都会创建一个新的 symbol类型：

```js
Symbol("foo") === Symbol("foo"); // false
```

```js
var sym = new Symbol(); // TypeError
```

​	

