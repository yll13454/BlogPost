```js
_.chunk(array, [size=1])
```

> 将数组（array）拆分成多个 `size` 长度的区块，并将这些区块组成一个新数组。 如果`array` 无法被分割成全部等长的区块，那么最后剩余的元素将组成一个区块。

#### 参数

1. `array` *(Array)*: 需要处理的数组
2. `[size=1]` *(number)*: 每个数组区块的长度

#### 例子：

```js
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]
 
_.chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]
```

### 源码解析

**`Math.ceil()`** 函数返回大于或等于一个给定数字的最小整数。

```js
console.log(Math.ceil(.95));
// expected output: 1

console.log(Math.ceil(4));
// expected output: 4
```

**new Array()**

![img](D:\笔记\lodashjs源码\media\42c9a686c122431bbb1a22b84bfe9eaa~tplv-k3u1fbpfcp-watermark.image)

创建新数组的几种方法

- `Array(3).fill({})`指定某个值来填充数组.可用于简单类型填充创建，换做复杂类型值, 修改每一个 `{}` , 都会影响其它的 `{}`. 因为它们都是对同一个对象的引用.
- Array(3).map(() => {})

```js
let studentRow = Array(3).map(() => {});
// > studentRow
// [ <3 empty items> ]
let arr = Array(3);
// > arr
// [ <3 empty items> ]
```

**警告:**

- 如若一个数组没有任何单元, 但它的 length 属性中却显示有单元数量, 这样奇特的数据结构会导致一些怪异的行为. 我们将包含至少一个 “空单元” 的数组称之为 “稀疏数组”. undefined 单元非 “空单元”.
- 永远不要创建和使用空单元数组.

`{}` 在这里被视作语法块了, 没有任何意义. 

```js
let studentRow = Array(3).fill(undefined).map(() => Object.create(null));
studentRow[0].name = 'tony';
// > studentRow
// [ { name: 'tony' }, {}, {} ]
```

`{...}` 里面的代码会被解析为一系列语句. `{}` 也因此不能达到我们预期的结果. 所以, 我们可以用 `(...)` 将 `{}` 包装成表达式, 即 `({})`.

```
let studentRow = Array(3).fill(undefined).map(() => ({}));
studentRow[0].name = 'tony';
// > studentRow
// [ { name: 'tony' }, {}, {} ]
```

- `Function.prototype.apply()`

```js
let studentRow = Array.apply(null, {length: 3}).map(() => ({}));
studentRow[0].name = 'tony';
// > studentRow
// [ { name: 'tony' }, {}, {} ]
```



**源码**

```js
function _chunk(array,size){
	size = Math.max(size,0);
	const length = array == null ? 0 : array.length;
	if (!length||size<1) {
		return []
	}
	let index = 0
	let resIndex = 0
	const result = new Array(Math.ceil(length/size))
	while(index<length){
		result[resIndex++] = Array.slice(index,index+size)
	}
	return result;
}
```

