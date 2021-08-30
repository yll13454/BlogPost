首先我们应该明确一点：ECMAScript中所有函数的参数都**是按值来传递**的。

- 原始值：只是把变量里的值传递给参数，之后参数和这个变量互不影响。
- 引用值：对象变量它里面的值是这个对象在堆内存中的内存地址，这一点你要时刻铭记在心！因此它传递的值也就是这个内存地址，这也就是为什么函数内部对这个参数的修改会体现在外部的原因了，因为它们都指向同一个对象呀。

**我们可以把ECMAScript函数的参数想象成局部变量。在向参数传递基本类型的值时，被传递的值被复制给一个局部变量（即命名参数，或者用ECMAScript的概念来说，就是arguments对象中的一个元素）。在向参数传递引用类型时，会把这个值在内存中的地址（指针）复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。**

### **按引用传递**

分为俩种，一种是函数内部赋值，是值赋值，还是引用赋值

值赋值

```js
function setName(obj) {
    obj.name = "Nicholas";
}

var person = new Object();
setName(person);   // obj = person
alert(person.name);   // "Nicholas" 看起来是按引用传递，但千万不要以为是按引用传递~~~
```

引用赋值

```js
function setName(obj) {
2     obj.name = "Nicholas";
3     obj = new Object(); //改变obj的指向，此时obj指向一个新的内存地址，不再和person指向同一个
4     obj.name = "Greg";
5 }
6 
7 var person = new Object();
8 setName(person);  //你看看下面，相信我也是按值传递的了吧
9 alert(person.name);  //"Nicholas"
```

**引用传递的是指针的值，obj=new Object()改写了自己的指向，并不会影响到person的指向，这种方式就是按引用传递。即使不把对象赋值给函数的参数，obj改写指向对person也没影响，**

