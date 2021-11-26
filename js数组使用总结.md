# js数组使用总结

[js数组使用总结](https://juejin.cn/post/6918316914569641997)

数组是一种特殊对象。js中并没有真正的数组，只是用对象模拟数组。

![img](media/2741b5d216d54014a66f6b95056aa305tplv-k3u1fbpfcp-watermark.awebp) 

查看对象属性的方法在数组身上也同样适用。**值得注意的是:数组的下标类型为字符串，并不是数字。**

**js的数组**

- 元素的数据类型可以不同
- 内存不一定是连续的(对象是随机存储的)
- 不能通过数字下标访问，而是通过字符串下标进行访问。(这意味着数组可以有任何key)

## 创建数组

创建数组有两种方式，分别是:

```js
  let arr=[1,2,3]
  let arr=new Array(1,2,3)
```

## 字符串转化为数组

- split()
- Array.from()

![img](media/d47a5d91f9324094b1ee74873c1280a8tplv-k3u1fbpfcp-watermark.awebp) 

## 伪数组

没有数组共有属性的数组就是伪数组(伪数组的原型链中并没有数组的原型)

![img](media/d9b4a385ab4640249235f3084a61f20etplv-k3u1fbpfcp-watermark.awebp) 伪数组中并没有push,pop等方法(通过`console.dir(divList)`可看出)，我们可以通过`Array.from()`来转化
![img](media/6c7075152b3744f3aa15720eff0b4fabtplv-k3u1fbpfcp-watermark.awebp)
转化之后就可以成功的push啦

## 合并两个数组

- concat()

此方法不会改变原数组

```js
  let arr1=[1,2,3]
  let arr2=[4,5,6]
  arr1.concat(arr2)//[1,2,3,4,5,6]
  arr1//[1,2,3]
  arr2//[4,5,6]
```

## 截取数组

- slice()

此方法不会改变原数组

```js
let arr=[1,2,3,4,5,6]
arr.slice(3) //[4,5,6]
arr//[1,2,3,4,5,6]
```

## 删数组元素

- 删头部元素:`arr.shift()`arr会被修改，并返回被删元素

```js
  let arr=[1,2,3,4,5,6]
  arr.shift()//1
  arr//[2, 3, 4, 5, 6]
```

- 删尾部元素:`arr.pop()`arr会被修改，并返回被删元素

```js
  let arr=[1,2,3,4,5,6]
  arr.shift()//6
  arr//[1, 2, 3, 4, 5]
```

- 删中间:
   `arr.splice(index,1)`//删除index的第一个元素,返回被删元素
   `arr.splice(index,1,'x')`//在删除位置添加'x' ,返回被删元素
   `arr.splice(index,1,'x','y')`//在删除位置添加'x'和'y',返回被删元素

```js
 let arr=[1,2,3,4,5,6,7,8,9]
 //删除元素4
 arr.splice(3,1)//4
 arr//[1, 2, 3, 5, 6, 7, 8, 9]
 
 let arr=[1, 2, 3, 5, 6, 7, 8, 9]
 //删除下标3并添加3.5和4
 arr.splice(2,1,3.5,4)//3
 arr//[1, 2, 3.5, 4, 6, 7, 8, 9]
```

## 查看数组元素

### 查看属性

- `Object.keys(arr)`
- `Object.values(arr)`

```
let arr=[1,2,3,4,5]
arr.x='xxx'
Object.keys(arr)//["0", "1", "2", "3", "4", "x"]
Object.values(arr)// [1, 2, 3, 4, 5, "xxx"]
复制代码
```

- `for in`循环

![img](media/54834c473b034948ab9981e3c21b3235tplv-k3u1fbpfcp-watermark.awebp)

### 查看只含数字的元素

- `for循环`

![img](media/8ade7edfd643460e9b55e518e617513ctplv-k3u1fbpfcp-watermark.awebp)

- `forEach循环`

![img](media/94d9f6296acd4107b566cc053d2b1162tplv-k3u1fbpfcp-watermark.awebp)

### 查看某个元素是否在数组里

- `arr.indexOf(item)`存在则返回数组索引，否则返回-1

```
let arr=[1,2,3,4,5,6]
arr.indexOf(2)//1
arr.indexOf(7)//-1
```

### 使用条件查找元素

```
//查找第一个为偶数的元素
let arr=[1,2,3,4,5,6]
arr.find(item=>item%2===0)//2

//查找第一个为偶数的下标
let arr=[1,2,3,4,5,6]
arr.findIndex(item=>item%2===0)//1
```

## 增加数组中的元素

- 在尾部添加:`arr.push(item1,item2)`

```
  let arr=[3,4,5,6]
  arr.push(7,8,9)
  arr//[3,4,5,6,7,8,9]
```

- 在头部添加:`arr.unshift(item1,item2)`

```
  let arr=[3,4,5,6]
  arr.unshift(1,2,3)
  arr//[1, 2, 3, 3, 4, 5, 6]
```

- 在中间添加:`arr.splice(index,0,'x')`

```
  let arr=[1,2,3,4,5,6,7]
  //在下标为2的位置添加3.33,3.44
  arr.splice(2,0,3.33,3.44)//[1, 2, 3.33, 3.44, 3, 4, 5, 6, 7]
```

![数组方法大全](media/16cd6d6deb3a6f8ftplv-t2oaga2asx-watermark.awebp) 