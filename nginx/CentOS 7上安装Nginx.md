# [2019 年如何在 CentOS 7 上安装最新版 Nginx](https://segmentfault.com/a/1190000018109309)

启动 Nginx

```crmsh
sudo systemctl start nginx
```

停止 Nginx

```arduino
sudo systemctl stop nginx
```

重启 Nginx

```ebnf
sudo systemctl restart nginx
```

修改 Nginx 配置后，重新加载

```ebnf
sudo systemctl reload nginx
```

设置开机启动 Nginx

```routeros
sudo systemctl enable nginx
```

关闭开机启动 Nginx

```routeros
sudo systemctl disable nginx
```

