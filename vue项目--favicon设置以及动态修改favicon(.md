# vue项目--favicon设置以及动态修改favicon(浏览器顶部小图标)

https://blog.csdn.net/weixin_43953710/article/details/102953903



[favicon.ico不显示的原因分析和解决办法](http://www.360doc.com/content/16/0416/10/6828497_551049460.shtml)

根目录.htaccess配置文件为:

 

```
RewriteEngine on
rewritecond %{http_host} ^joyfox.cn [nc]
rewriterule ^(.*)$ http://www.joyfox.cn/$1 [r=301,nc]
ErrorDocument 404 /404.html
```

  后来改成:

```
<link rel="shortcut icon" type="image/ico" href="http://joyfox.cn/favicon.ico"></link>
```

 

  

  呵呵，就可以了，究其缘故，估计是顶级域名跳转的配置所致。




  所以如果一切设置都没问题，仍不能显示favicon.ico的话，不妨把www去掉试试

 