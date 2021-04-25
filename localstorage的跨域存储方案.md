# localstorage的跨域存储方案

localStorage和 sessionStorage的主要区别是：localStorage的生命周期是永久的，意思就是如果不主动清除，存储的数据将一直被保存。而sessionStorage顾名思义是针对一个session的数据存储，生命周期为当前窗口，一旦窗口关闭，那么存储的数据将被清空。

```csharp
#localStorage和sessionStorage的一些方法：
#添加键值对： setItem(key,value);
#获取键值对： getItem(key);
#删除键值对： removeItem(key);
#清除所有键值对： clear();
#获取属性名称（键名称）： key(index);
#获取键值对的数量： length;

#localStorage 的存取方式
localStorage.age = 88; // 用localStorage属性的方式来添加条目
localStorage.setItem("animal","cat"); // 推荐使用setItem的方式存储一个名为animal，值为cat的数据
var animal = localStorage.getItem("animal"); //读取本地存储中名为animal的数据，并赋值给变量animal
console.log(animal);  
localStorage.removeItem("animal"); //删除单条数据
localStorage.clear(); //清除所有数据

#sessionStorage 的存取方式
sessionStorage.work = "police";
sessionStorage.setItem("person", "Li Lei");
var person = sessionStorage.getItem("person");
console.log(person);
```

不同浏览器无法共享localStorage和sessionStorage中的信息。同一浏览器的相同域名和端口的不同页面间可以共享相同的 localStorage，但是不同页面间无法共享sessionStorage的信息。

| URL                                                          | 说明                           | 是否允许通信                           |
| :----------------------------------------------------------- | :----------------------------- | :------------------------------------- |
| [http://www.a.com/a.js](https://link.jianshu.com?t=http://www.a.com/a.js)   [http://www.a.com/b.js](https://link.jianshu.com?t=http://www.a.com/b.js) | 同一域名下                     | 允许                                   |
| [http://www.a.com/lab/a.js](https://link.jianshu.com?t=http://www.a.com/lab/a.js)   [http://www.a.com/script/b.js](https://link.jianshu.com?t=http://www.a.com/script/b.js) | 同一域名下不同文件夹           | 允许                                   |
| [http://www.a.com:8000/a.js](https://link.jianshu.com?t=http://www.a.com:8000/a.js)   [http://www.a.com/b.js](https://link.jianshu.com?t=http://www.a.com/b.js) | 同一域名，不同端口             | 不允许                                 |
| [http://www.a.com/a.js](https://link.jianshu.com?t=http://www.a.com/a.js)   [https://www.a.com/b.js](https://link.jianshu.com?t=https://www.a.com/b.js) | 同一域名，不同协议             | 不允许                                 |
| [http://www.a.com/a.js](https://link.jianshu.com?t=http://www.a.com/a.js)   [http://70.32.92.74/b.js](https://link.jianshu.com?t=http://70.32.92.74/b.js) | 域名和域名对应ip               | 不允许                                 |
| [http://www.a.com/a.js](https://link.jianshu.com?t=http://www.a.com/a.js)   [http://script.a.com/b.js](https://link.jianshu.com?t=http://script.a.com/b.js) | 主域相同，子域不同             | 不允许                                 |
| [http://www.a.com/a.js](https://link.jianshu.com?t=http://www.a.com/a.js)   [http://file.a.com/b.js](https://link.jianshu.com?t=http://file.a.com/b.js) | 同一域名，不同二级域名（同上） | 不允许（cookie这种情况下也不允许访问） |
| [http://www.cnblogs.com/a.js](https://link.jianshu.com?t=http://www.cnblogs.com/a.js)   [http://www.a.com/b.js](https://link.jianshu.com?t=http://www.a.com/b.js) | 不同域名                       | 不允许                                 |

