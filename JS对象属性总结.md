[JS对象属性总结](https://juejin.cn/post/6977609099500322824)

## 按来源分类

- 自有属性
- 原型属性

> in操作符可以检查对象上所有的自有属性和原型属性，包括Symbol值。
>
> hasOwnProperty方法可以判断某个属性是否是该对象的自有属性，包括Symbol值。

```
let s1 = Symbol('s1');

function Fun() {
    this.innerValue = '自有属性';
    this[s1] = 'Symbol属性'
}
Fun.prototype.proValue = "原型属性";

let obj = new Fun()

console.log('innerValue' in obj);
console.log('proValue' in obj);
console.log(s1 in obj);

console.log('-----------------');

console.log(obj.hasOwnProperty('innerValue'));
console.log(obj.hasOwnProperty(s1));
console.log(obj.hasOwnProperty('proValue'));


//执行结果
true
true
true
-----------------
true
true
false
```

### 属性枚举

> 可以枚举对象上所有的可枚举的自有属性和原型属性(不包括Symbol值)。
>
> Object.keys只枚举自有属性(不包括Symbol值)。
>
> Object.getOwnPropertyNames返回一个指定对象所有的自身属性（包括不可枚举属性，但不包括Symbol值）的数组。
>
> Object.getOwnPropertySymbols返回一个指定对象所有的Symbol值。
>
> Reflect.ownKeys可以返回一个对象自身的所有属性，包括Symbol属性

```js
function Fun() {
    this.innerValue = '自有属性';
}
Fun.prototype.proValue = "原型属性";

let obj = new Fun();
Object.defineProperty(obj, 'noEnum', {
    value: '不可枚举属性'
})
let s1 = Symbol('s1');
obj[s1] = 'symbol属性'

let forInArr = [];
for(let key in obj) {
    forInArr.push(key);
}

console.log(forInArr);
console.log(Object.keys(obj));
console.log(Object.getOwnPropertyNames(obj));
console.log(Reflect.ownKeys(obj))

//执行结果
[ 'innerValue', 'proValue' ]
[ 'innerValue' ]
["innerValue", "noEnum"]
["innerValue", "noEnum", Symbol(s1)]
```

####  Object.getOwnPropertyDescriptor(obj, propName) //返回对象obj的propName属性的自身属性(非集成)的属性描述符

##### Object.freeze()

> Object.freeze()会创建一个冻结对象。一旦对象被冻结，则不能再向里边添加新的属性；不能删除已有的属性；不能修改该对象已有属性的特征属性（可枚举，可配置，可写等）；不能修改已有属性的值。
>
> Object.isFrozen()方法用来判断一个对象是否被冻结。

##### Object.seal()

> Object.seal()封闭一个对象。阻止添加新属性；将所有的现有属性标记为不可配置；当前属性如果原来是可写的，则可以改变。
>
> Object.isSealed()方法用来判断一个对象是否被密封。

##### Object.preventExtensions()

> Object.preventExtensions()让一个对象变得不可扩展，永远不能再添加新属性。（如果一个对象可以添加新的属性，则这个对象是扩展的。）一旦被标记为不可扩展的，则不能再变为可扩展；Object.preventExtensions()仅阻止添加自身属性，其对象原型不受影响；不可扩展的对象属性仍然可以删除。
>
> Object.isExtensible()方法用来判断一个对象是否可扩展。

Object.freeze, Object.seal, Object.preventExtensions方法都返回原对象，不会创建副本。

##### ES6开始，[[Prototype]]可以通过Object.getPrototypeOf和Object.setPrototypeOf访问，这个等同于Javascript的非标准但许多浏览器实现的属性__proto__

作者：luochen123
链接：https://juejin.cn/post/6977609099500322824
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

