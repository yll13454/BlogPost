## **Nginx 配置浏览器缓存**

- 强缓存

- - <- cache-control: max-age=600 ---- no-store 不缓存 no-cache 不使用强缓存
  - <- expires: Mon, 14 Sep 2020 09:02:20 GMT



- 协商缓存

- - <- last-modified: Fri, 07 Aug 2020 02:35:59 GMT
  - -> if-modified-since: Fri, 07 Aug 2020 02:35:59 GMT
  - <- etag: W/“5f2cbe0f-2382"
  - -> if-none-match: W/"5f2cbe0f-2382"



### **Nginx 配置**

- gzip 和 etag

```yaml
http {
  # 开启gzip
  gzip on;
  # 启用gzip压缩的最小文件；小于设置值的文件将不会被压缩
  gzip_min_length 1k;
  # gzip 压缩级别 1-9
  gzip_comp_level 5;
  # 进行压缩的文件类型。
  gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
  # 是否在http header中添加Vary: Accept-Encoding，建议开启
  gzip_vary on;
  
  etag on;
}
```

- 强缓存配置

```text
server {
  location ~* \.(html)$ {
    access_log off;
    add_header  Cache-Control  max-age=no-cache;
  }

  location ~* \.(css|js|png|jpg|jpeg|gif|gz|svg|mp4|ogg|ogv|webm|htc|xml|woff)$ {
    access_log off;
    add_header    Cache-Control  max-age=360000;
  }
}

```

### **HTTPS 配置**

- [https://buy.cloud.tencent.com/ssl](https://link.zhihu.com/?target=https%3A//buy.cloud.tencent.com/ssl)
- **HTTPS 域名还需要配置！！**

```text
ssl_certificate "/etc/pki/nginx/server.crt";
ssl_certificate_key "/etc/pki/nginx/private/server.key";
```

- 安全组规则里打开 443 端口

- HTTP/2 演示

- - [https://http2.akamai.com/demo](https://link.zhihu.com/?target=https%3A//http2.akamai.com/demo)
  - 链路复用
  - 压缩请求头



```text
return 301 https://www.nllcoder.com$request_uri;
server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  www.nllcoder.com;
        root         /usr/share/nginx/html;

        ssl_certificate "/etc/pki/nginx/www.nllcoder.com_bundle.crt";
        ssl_certificate_key "/etc/pki/nginx/private/www.nllcoder.com.key";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers PROFILE=SYSTEM;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

```



反向代理配置

```text
location /api/ {
    proxy_pass http://realworld.api.fed.lagounews.com/api/;
}

# ^~ 以/开头的内容
location ^~ / {
    # 注意这里末尾要有 /，会把请求的路径拼接过来
    proxy_pass http://127.0.0.1:3000/;
}
location /api/ {
  add_header Access-Control-Allow-Credentials true;
  add_header Access-Control-Allow-Origin $http_origin;
  add_header Access-Control-Allow-Methods 'GET, POST, PUT, PATCH, DELETE, OPTIONS';
  add_header Access-Control-Allow-Headers 'Authorization, Content-Type, Accept, Origin, User-Agent, DNT, Cache-Control, X-Mx-ReqToken, X-Requested-With';
  add_header Access-Control-Max-Age 86400;
  proxy_pass http://realworld.api.fed.lagounews.com/api/;
}
```