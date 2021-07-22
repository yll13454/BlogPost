```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <div id="div1">

    </div>
    <input value="加张图片" id="img" type="button">
    <script>
        var img=document.getElementById("img");                 // 先导航到INPUT标签
        img.onclick=function () {                               // 给他绑定onclick事件
            var ele=document.getElementById("div1");            // 找到要操作的标签 div1
            var add=document.createElement("img");              //  创建一个IMG的标签

            add.src="timg.jpg";                                 // 给他添加src属性 链接到文件名(这个是动态HTML写法 微软改的)
           　　//add.setAttribute("img","timg,jpg");                      // 这是第2种添加方法
            ele.appendChild(add);                               // 给div1加入 IMG标签

        }
    </script>
</body>
</html>
```

```
给Html元素添加方法

//创建一个textarea

std2area=document.createElement("textarea");

//设置属性

std2area.setAttribute("id","member_Task"+position);

std2area.setAttribute("name","member_Task"+position);

//添加方法

std2area.attachEvent("onblur",CheckNewNull("member_Task"+position));

 

如果添加方法写成：std2area.attachEvent("onblur",CheckNull("member_Task"+position));

或者写成std2area.attachEvent("onblur",CheckNull);

那么会出现错误。原因是这样做是将CheckNull("member_Task"+position)的返回值赋给onblur

而不是将函数CheckNull赋给该元素。所以正确的做法是将CheckNull用一个函数返回，如上所示。

 

最后写写添加方法的其它途径：

 

  Std2area.onblur=CheckNull;

  Std2area.setAttribute(“onblur”,CheckNull);
```

**JS获取DOM元素的方法（8种）**

- 通过ID获取（getElementById）
- 通过name属性（getElementsByName）
- 通过标签名（getElementsByTagName）
- 通过类名（getElementsByClassName）
- 通过选择器获取一个元素（querySelector）
- 通过选择器获取一组元素（querySelectorAll）
- 获取html的方法（document.documentElement）
- document.documentElement是专门获取html这个标签的
- 获取body的方法（document.body）
- document.body是专门获取body这个标签的。
