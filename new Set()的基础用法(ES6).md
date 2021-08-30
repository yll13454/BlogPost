# 1、什么是Set()

Set是es6新增的数据结构，**似于数组**，但它的一大特性就是**所有元素都是唯一的**，没有重复的值，我们一般称为集合。

Set本身是一个构造函数，用来生成 Set 数据结构

# 2、增删改查方法

###### 2.1 添加元素`add`

添加某个值，返回 Set 结构本身，当添加实例中已经存在的元素，set不会进行处理添加

```cpp
let list=new Set();
list.add(1)
list.add(2).add(3).add(3)   // 2只被添加了一次
123
```

###### 2.2 删除元素 `delete`

删除某个值，返回一个布尔值，表示删除是否成功

```
let list=new Set([1,20,30,40])
list.delete(30)      //删除值为30的元素，这里的30并非下标
12
```

###### 2.3 判断某元素是否存在`has`

返回一个布尔值，判断该值是否为Set的成员

```
let list=new Set([1,2,3,4])
list.has(2)//true
12
```

###### 2.4 清除所有元素`clear`

清除所有成员，没有返回值

```
let list=new Set([1,2,3,4])
list.clear()
12
```

# 3、遍历方法

###### 3.1 遍历 `keys()`

返回键名的遍历器，相等于返回键值遍历器values()

```
let list2=new Set(['a','b','c'])
for(let key of list2.keys()){
   console.log(key)//a,b,c
}
1234
```

###### 3.2 遍历 `values()`

返回键值的遍历器

```
let list=new Set(['a','b','c'])
for(let value of list.values()){
console.log(value)//a,b,c
}
1234
```

###### 3.3 遍历 `entries()`

返回键值对的遍历器

```
let list=new Set(['4','5','hello'])
for (let item of list.entries()) {
  console.log(item);
}
// ['4','4']   ['5','5']   ['hello','hello'] 
12345
```

###### 3.4 遍历 `forEach()`

使用回调函数遍历每个成员

```
let list=new Set(['4','5','hello'])
list.forEach((value, key) => console.log(key + ' : ' + value))
// 4:4    5:5   hello:hello
123
```

# 4、使用情形

###### 4.1 用于数组去重

```cpp
let arr = [3, 5, 2, 2, 5, 5];
let setArr = new Set(arr)     // 返回set数据结构  Set(3) {3, 5, 2}

//方法一   es6的...解构
let unique1 =  [...setArr ];      //去重转数组后  [3,5,2]

//方法二  Array.from()解析类数组为数组
let unique2 = Array.from(setArr )   //去重转数组后  [3,5,2]
12345678
```

###### 4.2 用于字符串去重

```cpp
let str = "352255";
let unique = [...new Set(str)].join("");     // 352 
12
```

###### 4.3 实现并集、交集、和差集

```cpp
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// （a 相对于 b 的）差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```