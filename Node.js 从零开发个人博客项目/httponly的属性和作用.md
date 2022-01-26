**什么是HttpOnly?**

如果cookie中设置了HttpOnly属性，那么通过js脚本将无法读取到cookie信息，这样能有效的防止XSS攻击，窃取cookie内容，这样就增加了cookie的安全性，即便是这样，也不要将重要信息存入cookie。

**HttpOnly的设置样例**

```js
response.setHeader(``"Set-Cookie"``, "cookiename=httponlyTest;Path=/;Domain=domainvalue;Max-Age=seconds;HTTPOnly");
```

例如：

```js
//设置cookie
response.addHeader("Set-Cookie", "uid=112; Path=/; HttpOnly")
//设置多个cookie
response.addHeader("Set-Cookie", "uid=112; Path=/; HttpOnly");
response.addHeader("Set-Cookie", "timeout=30; Path=/test; HttpOnly");
//设置https的cookie
response.addHeader("Set-Cookie", "uid=112; Path=/; Secure; HttpOnly");
```

**具体参数的含义再次不做阐述，设置完毕后通过js脚本是读不到该cookie的，但使用如下方式可以读取。**

```js
Cookie cookies[]=request.getCookies();
```



