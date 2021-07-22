## 首页白屏

## 路由跳转没问题，刷新变成404

### 第一种

需要变更nginx配置

```
server {
        listen      9091;
        server_name localhost;
        root   C:\Users\Administrator\Desktop\ftp_files\dist;
        index  index.html;
        location / {
	       try_files $uri $uri/ /index.html;
        }
    }
```

第二种情形

**vue中配置：**

   \1. router中的配置:
![img](media/16db3c9a9ae80cdd)
   \2. config/index.js中的配置

![img]()![img](media/16db3ca691ad53e0)

**nginx中配置：**

其中location的路由/history和vue-router中路由的base要一致，

项目所在文件夹的名称为history，和location要一致

```
server {
    listen 443 ssl;
    server_name xxx.com;  # localhost修改为您证书绑定的域名。
    
    root C:\Users\Administrator\Desktop\demos;
    index index.html index.htm;
    
    location / {
    }

    location /history {
        try_files $uri $uri/ /history/index.html;
    }
}复制代码
```

最终访问地址：xxx.com/history

### 刷新变成404**解决方案：**

对于VUE的router[mode: history]模式在开发的时候，一般都不出问题。是因为开发时用的服务器为node，Dev环境中自然已配置好了。

但对于放到nginx下运行的时候，自然还会有其他注意的地方。总结如下：

在nginx里配置了以下配置后， 可能首页没有问题，但链接其他会出现（404）

```
location / {`` ``root D:\Test\exprice\dist;`` ``index index.html index.htm;`` ``try_files $uri $uri/ ``/index``.html;`` ``add_header ``'Access-Control-Allow-Origin'` `'*'``;`` ``add_header ``'Access-Control-Allow-Credentials'` `'true'``;`` ``add_header ``'Access-Control-Allow-Methods'` `'GET, POST, OPTIONS'``;`` ``add_header ``'Access-Control-Allow-Headers'` `'DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type'``;``}` `location ^~``/api/` `{`` ``proxy_pass http:``//39``.105.109.245:8080/;``}
```

**为了解决404，需要通过以下两种方式：**

1、官网推荐

```
location / {``　　root D:\Test\exprice\dist;``　　index index.html index.htm;``　　try_files $uri $uri/ ``/index``.html;
```

2、匹配errpr_page

```
location /{``　　root ``/data/nginx/html``;``　　index index.html index.htm;``　　error_page 404 ``/index``.html;``}
```

3、 （vue.js官方教程里提到的https://router.vuejs.org/zh-cn/essentials/history-mode.html）

```
server {``  ``listen  8888;``#默认端口是80，如果端口没被占用可以不用修改``  ``server_name localhost;``  ``root  E:``/vue/my_project/dist``;``#vue项目的打包后的dist``  ``location / {``   ``try_files $uri $uri/ @router;``#需要指向下面的@router否则会出现vue的路由在nginx中刷新出现404``   ``index index.html index.htm;``  ``}``  ``#对应上面的@router，主要原因是路由的路径资源并不是一个真实的路径，所以无法找到具体的文件``  ``#因此需要rewrite到index.html中，然后交给路由在处理请求资源``  ``location @router {``   ``rewrite ^.*$ ``/index``.html last;``  ``}``  ``#.......其他部分省略``}
```

[vue在history模式下如何配置相关文件和后端ngnix配置](https://www.jianshu.com/p/d2a2ee602c13)

