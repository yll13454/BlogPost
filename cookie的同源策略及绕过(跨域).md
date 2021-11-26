## 一：cookie的"同源策略"和一般的同源策略 区别：

一般的同源策略：需要满足：协议 域名 端口
cookie的"同源策略"：**仅仅关注 域名**

## 二：cookie的domain属性：

domain决定了 访问页面的时候 哪些cookie是需要发送/可以保存
domain默认值为当前域名

表格：横向是domain的属性值，纵向是请求的网页/域名

| 域\cookie.domain         | molibird.com | .molibird.com | a.molibird.com | b.molibird.com |
| ------------------------ | ------------ | ------------- | -------------- | -------------- |
| molibird.com/index.php   | 可以         | 可以          | X              | X              |
| a.molibird.com/index.php | X            | 可以          | 可以           | X              |
| b.molibird.com/index.php | X            | 可以          | X              | 可以           |

## 三：绕过限制：

**即 设置cookie时 指定具体需要的domain(不用默认值)：**

### 设置domain的要求：

(1)父域名
 (2)"子"域名：所有子域名 包括 自己
 **注意：**
 无法修改到.com这类顶级域名
 无法修改到 具体的子域名 比如 molibird.com 修改到 a.molibird.com
 无法修改到"同级域名" 比如 a.molibird.com 修改到 b.molibird.com 是不行的

### 设置方法

例子：molibird.com/index.php

```javascript
document.cookie = "key=value;";  //默认值 domain = "molibird.ocm"
document.cookie = "key=value; domain=molibird.com;";  //domain = ".molibird.ocm"
document.cookie = "key=value; domain=.molibird.com;";  //domain = ".molibird.ocm"
//document.cookie = "key=value; domain=a.molibird.com;";  //无效 不可以设置 具体子域名
```

例子：a.molibird.com/index.php

```javascript
document.cookie = "key=value;";  //默认值 domain = "a.molibird.ocm"
document.cookie = "key=value; domain=molibird.com;";  //domain = ".molibird.ocm"
document.cookie = "key=value; domain=.molibird.com;";  //domain = ".molibird.ocm"
//document.cookie = "key=value; domain=b.molibird.com;";  //无效 不可以设置 "同级域名"
```

#### 注意：先设置 document.domain 再设置 document.cookie 不会起作用：

例子：a.molibird.com/index.php

```javascript
document.domain = 'molibird.com';
document.cookie = "key=value";  //domain = "a.molibird.com"  不会因为前面的设置而变化
```