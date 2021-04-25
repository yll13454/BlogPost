`vue-router`需要满足两个需求

1. 记录当前页面的状态
2. 可以使用浏览器的前进后退功能

而`vue-router`为了满足以上两个需求实现以下三个功能

1. 改变URL且不让浏览器向服务器发出请求
2. 检测URL的改变
3. 截获URL地址, 并解析出需要的信息来匹配路由规则

# hash模式的特点

hash表示的是地址栏URL中#符号(也称作为**锚点**), hash虽然会出现在URL中, 但是不会被包含在Http请求中, 因此hash值改变不会重新加载页面.

由于hash值变化不会引起浏览器向服务器发出请求, 而且hash改变会触发**hashchange事件**, 浏览器的进后退也能对其进行控制, 所以在HTML5之前, 基本都是使用hash来实现前端路由.

# history模式的特点

利用了HTML5新增的**`pushState()`和`replaceState()`**两个api, 通过这两个api完成URL跳转不会重新加载页面

同时history模式解决了hash模式存在的问题. hash的传参是基于URL的, 如果要传递复杂的数据, 会有体积限制, 而history模式不仅可以在URL里传参, 也可以将数据存放到一个特定的对象中

# vue-router实现

hash模式和history模式实现vue-router跳转api的区别

| api     | hash                    | history                     |
| ------- | ----------------------- | --------------------------- |
| push    | window.location.assign  | window.history.pushState    |
| replace | window.location.replace | window.history.replaceState |
| go      | window.history.go       | window.history.go           |
| back    | window.history.go(-1)   | window.history.go(-1)       |
| forward | window.history.go(1)    | window.history.go(1)        |

Browser包含window对象，Navigator对象，Screen对象，History对象，Location对象等，其中

**window对象**表示浏览器打开的窗口；

**Navigator对象**包含浏览器的相关信息，其常用的属性有navigator.userAgent获取浏览器内核等信息；

**Screen对象**包含客户端显示屏幕的信息，如screen.height或screen.width获取宽高；

**history对象**包含访问过的URL，是window对象的一部分，有三个方法，back(),forward(),go(),调用这三个方法，浏览器会加载对应页面；

**location对象**包含当前URL有关的信息，是window对象的一部分，常用的属性有location.hash返回URL的hash值，location.port返回当前使用的端口号。

> hash和history只是浏览器的两个特性

### hash

使用 **window.location.hash** 可以读取当前页面的hash值,也可以写入，**hashchange事件**可以响应hash的的改变，有两个特点：

1、改变hash值，浏览器不会重载页面，但会在历史访问里增加一条纪录 

2、刷新重载页面时，hash值不会传给服务器端，浏览器访问的URL是http://wenyan.51easymaster.com/#/WYHome， 实际上向服务器发起请求的URL是http://wenyan.51easymaster.com/



### history

HTML5 History Interface 中history对象新增了两个方法 **pushState()** 和 **popState()**。 这两个方法应用于浏览器的历史记录栈，它们提供了对历史记录进行修改的功能。只是当它们执行修改时，虽然改变了当前的 URL，但浏览器不会重载页面。

使用history模式要注意的是前端的 URL 必须和实际向后端发起请求的 URL 一致而且还需要后台配置支持，如访问http://wenyan.51easymaster.com/user/id 时如果后端缺少对 /user/id 的路由处理，将返回 404 错误。官方给出的方案是你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。



## **注意：如果 SEO 对你很重要，你应该使用 [`createWebHistory`](https://next.router.vuejs.org/zh/api/#createwebhistory)**。