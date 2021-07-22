# Object.keys 与 Reflect.ownKeys 的区别

Object.keys(obj) 返回结果是：一个由给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

而 Reflect.ownKeys(obj)的返回结果 等价于：

```
Object.getOwnPropertyNames(target).concat(Object.getOwnPropertySymbols(target)) 
```

Object.getOwnPropertyNames() 方法返回一个由指定对象的所有自身属性的属性名(包括不可枚举属性但不包括Symbol值作为名称的属性)组成的数组。

Object.getOwnPropertySymbols() 方法返回一个给定对象自身的所有 Symbol 属性的数组。

Reflect.ownKeys(obj) 等于把 给定的对象所有属性以数组的方式返回。

```js
var testObject = {[Symbol('1')]: 1, a: 2}; 
Object.defineProperty(testObject, 'myMethod', { 
    value: function () { 
        alert("Non enumerable property"); 
    }, 
    enumerable: false 
}); 
console.log(Object.keys(testObject)); 
// ['a'] 
console.log(Reflect.ownKeys(testObject)); 
// [ 'a', 'myMethod', Symbol(1) ] 
```

Object.keys能返回方法属性，只不过Object.keys返回的是可枚举属性，当设置对象的enumerable设置为false， 那就无法遍历。 

Reflect.ownKeys 返回所有的自己属性，不管可枚举还是不可枚举，所以可以遍历出方法属性。