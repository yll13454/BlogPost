对于Object.assign()而言, 

如果对象的属性值为简单类型（string， number），通过Object.assign({},srcObj);得到的新对象为‘深拷贝’；

如果属性值为对象或其它引用类型，那对于这个对象而言其实是浅拷贝的。这是Object.assign()特别值得注意的地方。




只有一个参数，`Object.assign`会直接返回该参数。

如果该参数不是对象，则会先转成对象，然后返回。

```
typeof Object.assign(2) // "object"
```

由于`undefined`和`null`无法转成对象，所以如果它们作为参数，就会报错。

```
Object.assign(undefined) // 报错
Object.assign(null) // 报错
复制代码
```

如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果`undefined`和`null`不在首参数，就不会报错。



```
const v1 = 'abc'
const v2 = true
const v3 = 10

const obj = Object.assign({}, v1, v2, v3)
obj // { "0": "a", "1": "b", "2": "c" }
```

上面代码中，`v1`、`v2`、`v3`分别是字符串、布尔值和数值，结果只有字符串合入目标对象（以字符数组的形式），数值和布尔值都会被忽略。这是因为只有字符串的包装对象，会产生可枚举属性。

```
Object.assign(true) // {[[PrimitiveValue]]: true}
Object.assign(10)  //  {[[PrimitiveValue]]: 10}
Object.assign('abc') // {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}
```

布尔值、数值、字符串分别转成对应的包装对象，可以看到它们的原始值都在包装对象的内部属性 `[[PrimitiveValue]]`上面，这个属性是不会被`Object.assign`拷贝的。只有字符串的包装对象，会产生可枚举的实义属性，那些属性则会被拷贝。



### `Object.assign`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。

```
Object.assign({b: 'c'},
  Object.defineProperty({}, 'invisible', {
    enumerable: false,//不可枚举
    value: 'hello'
  })
)
// { b: 'c' }
```

属性名为 `Symbol` 值的属性，也会被`Object.assign`拷贝。

```
Object.assign({ a: 'b' }, { [Symbol('c')]: 'd' })
// { a: 'b', Symbol(c): 'd' }
```

#### （2）同名属性的替换

同名属性，`Object.assign`的处理方法是替换，而不是添加。

```
const target = { a: { b: 'c', d: 'e' } }
const source = { a: { b: 'hello' } }
Object.assign(target, source)
// { a: { b: 'hello' } }
```

（3）数组的处理 `Object.assign`可以用来处理数组，但是会把数组视为对象。

```
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
```

## 用法

#### （1）为对象添加属性

#### （2）为对象添加方法

```
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
})

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
}
SomeClass.prototype.anotherMethod = function () {
  ···
}

```

上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用`assign`方法添加到`SomeClass.prototype`之中。

#### 3）克隆对象

```
function clone(origin) {
  return Object.assign({}, origin)
}
```

只能克隆原始对象自身的值，不能克隆它继承的值。

想要保持继承链，可以采用下面的代码。

```
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin)
  return Object.assign(Object.create(originProto), origin)
}
```

#### 4）合并多个对象

#### 5）为属性指定默认值