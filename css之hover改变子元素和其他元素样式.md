# [css之hover改变子元素和其他元素样式](https://www.cnblogs.com/zhaobao1830/p/6477964.html)

**参考地址：[链接](http://blog.csdn.net/u013047660/article/details/48026615)**

**+表示下一级元素，>表示子元素**

```
 1 <!DOCTYPE html>
 2 <html>
 3 <head lang="en">
 4     <meta charset="UTF-8">
 5     <title></title>
 6 </head>
 7 
 8 <style>
 9     #a {color : #FFFF00;}
10 
11     #a:hover + #c{color : #00FF00;}
12     #a:hover + #c > #b{color : #0000FF;}
13 </style>
14 <div id='a'>元素1
15 
16 </div>
17 <div id='c'>元素3
18     <div id='b'>元素2</div>
19 </div>
20 
21 
22 </html>
```

