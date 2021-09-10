## nginx禁用强缓存

在试试 *nginx* 禁用强缓存之后会发生什么效果。修改 *nginx* 配置文件：

```
server {
  listen       8080;
  server_name  localhost;
  location / {
     root  /Volumes/myFile/nginx_root;   
     index  index.html index.htm;
     add_header Cache-Control no-cache;
     # 为 public可以被任何对象缓存，private只能针对个人用户，而不能被代理服务器缓存
     add_header Cache-Control private;
  }
}
```

**Cache-Control**：**no-store**

禁止一切缓存（这个才是响应不被缓存的意思）。缓存通常会像非缓存代理服务器一样，向客户端转发一条 no-store 响应，然后删除对象。

**Cache-Control：no-cache**

强制客户端直接向服务器发送请求,也就是说每次请求都必须向服务器发送。服务器接收到请求，然后判断资源是否变更，是则返回新内容，否则返回304，未变更。这个很容易让人产生误解，使人误以为是响应不被缓存。实际上Cache-Control: no-cache是会被缓存的，只不过每次在向客户端（浏览器）提供响应数据时，缓存都要向服务器评估缓存响应的有效性。




**. Expires:**
设置以分钟为单位的绝对过期时间, 优先级比Cache-Control低, 同时设置Expires和Cache-Control则后者生效. **也就是说要注意一点: Cache-Control的优先级高于Expires**

expires起到控制页面缓存的作用，合理配置expires可以减少很多服务器的请求, expires的配置可以在http段中或者server段中或者location段中. 比如控制图片等过期时间为30天, 可以配置如下:
`location ~ .(gif|jpg|jpeg|png|bmp|ico)$ {`

```
root` `/var/www/img/``;
expires 30d;
}`
再比如:
`location ~ .(wma|wmv|asf|mp3|mmf|zip|rar|swf|flv)$ {
root` `/var/www/upload/``;
expires max;
}
```



[Nginx下关于缓存控制字段cache-control的配置说明](https://segmentfault.com/a/1190000037487315)

