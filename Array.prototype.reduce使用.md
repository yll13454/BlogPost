# Array.prototype.reduce

### 语法

`reduce` 接收 2 个参数： 第一个参数是回调函数（必选），第二个参数是初始值 `initialValue`（可选） 。

而第一个参数（回调函数），接收下面四个参数：

- Accumulator (acc) (累计器)
- Current Value (cur) (当前值)
- Current Index (idx) (当前索引)
- Source Array (src) (源数组)

#### 不带初始值

```javascript
[1,2,3,4].reduce((acc, cur) => {
  return acc + cur
})
// 1 + 2 + 3 + 4
// 10
复制代码
```

#### 带初始值

```javascript
[1,2,3,4].reduce((acc, cur) => {
  return acc + cur
}, 10)
// 10 + 1 + 2 + 3 + 4
// 20
```

**初始值 `initialValue` 可以是任意类型。如果没有提供 `initialValue`，`reduce` 会从索引 1 的地方开始执行 `callback` 方法，跳过第一个索引。如果提供 `initialValue`，从索引 0 开始。**

```
[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
  return accumulator + currentValue;
});
```

> 例子

### [数组里所有值的和](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#数组里所有值的和)

```
var sum = [0, 1, 2, 3].reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, 0);
// 和为 6
```

### [累加对象数组里的值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#累加对象数组里的值)

```
var initialValue = 0;
var sum = [{x: 1}, {x:2}, {x:3}].reduce(function (accumulator, currentValue) {
    return accumulator + currentValue.x;
},initialValue)

console.log(sum) // logs 6
```

### [将二维数组转化为一维](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#将二维数组转化为一维)

```
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  function(a, b) {
    return a.concat(b);
  },
  []
);
// flattened is [0, 1, 2, 3, 4, 5]
```

### [计算数组中每个元素出现的次数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#计算数组中每个元素出现的次数)

```
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) {
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

### [按属性对object分类](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce#按属性对object分类)

```
var people = [
  { name: 'Alice', age: 21 },
  { name: 'Max', age: 20 },
  { name: 'Jane', age: 20 }
];

function groupBy(objectArray, property) {
  return objectArray.reduce(function (acc, obj) {
    var key = obj[property];
    if (!acc[key]) {
      acc[key] = [];
    }
    acc[key].push(obj);
    return acc;
  }, {});
}

var groupedPeople = groupBy(people, 'age');
// groupedPeople is:
// {
//   20: [
//     { name: 'Max', age: 20 },
//     { name: 'Jane', age: 20 }
//   ],
//   21: [{ name: 'Alice', age: 21 }]
// }
```

#### **重塑**



