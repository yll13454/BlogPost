### `_.compact(array)`

创建一个新数组，包含原数组中所有的非假值元素。例如`false`, `null`,`0`, `""`, `undefined`, 和 `NaN` 都是被认为是“假值”。

例子：

```
_.compact([0, 1, false, 2, '', 3]);
// => [1, 2, 3]
```

## 密集数组VS稀疏数组

> 稀疏数组就是包含从0开始的不连续索引的数组。通常，数组的length属性值代表数组中元素的个数。如果数组是稀疏的，length属性值大于元素的个数。

```js
var sparse = new Array(10)
var dense = new Array(10).fill(undefined)
```

其中 `sparse` 的 `length` 为10，但是 `sparse` 数组中没有元素，是稀疏数组；而 `dense` 每个位置都是有元素的，虽然每个元素都为`undefined`，为密集数组 。

#### 区别

稀疏数组在迭代的时候会跳过不存在的元素。

```
sparse.forEach(function(item){
  console.log(item)
})
dense.forEach(function(item){
  console.log(item)
})
```

`sparse` 根本不会调用 `console.log` 打印任何东西，但是 `dense` 会打印出10个 `undefined` 。

### 数组循环for,for in,for of区别

- for循环

对于稀疏数组，容易造成无效循环。

- for in循环

**for...in语句**以任意顺序遍历一个对象的[可枚举属性](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。

1>循环顺序不定。

2>会遍历所有可枚举属性，包括继承属性。

例子

```js
var arr = [1,2,3]
arr.foo = 'foo'
for (let index in arr) {
  console.log(index)
}
//1,2,3,foo
```

- for of循环

调用原型链上的迭代器方法

使用 `for...of` 来遍历数组是安全的，因为这个方法是数组的原生方法，而且使用 `for...of` 来遍历同样不会遍历数组中稀疏数部分。

### 源码

code

```js
function compact(array){
	const resIndex = 0
	const result = []

	if(array == null){
		return result
	}

	for(const value of array){
		if(value){
			result[resIndex++]=value
		}
	}
	return result
}

```

