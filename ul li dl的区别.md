#### 区别：

- `<ul>`是项目列表，`<li>`做列表项，每一项的符号默认是小黑圆点；
   `<ol>`是编号列表，`<li>`做列表项，每一项的符号默认是数字;
   `<li>`是 list item 即列表项，但列表有很两种，所以外面得有 `<ul>` 或者 `<ol>` 用来区别无序列表（小点点）和有序列表（1,2,3...）。
- ul与ol前的符号是可以修改的。只需要修改它们的type值。
   ul的type属性有：disc—实心圆(默认）、circle—空心圆、square—实心方块
   ol的type属性有：1—数字（默认）、a—小写字母、A—大写字母、i—小写希腊字母、I—大写希腊字母。
   通过CSS可以在将它们前的符号改为图片；
- 定义列表定义一系列**项目**，同时定义项目的**描述**。
   正是定义列表有了项目描述，所以和无序列表区别就在此。

#### `<ul> <li>`代码的格式化

- 运用CSS格式化列表符：
   ul li{ list-style-type:none; }
- 将列表符换成图像：
   ul li{ list-style-type:none; list-style-image: url(/blog/images/icon.gif); }
- 左对齐:
   ul{ list-style-type:none; margin:0px; }
- 给列表加背景色:
   ul{ list-style-type: none; margin:0px; } ul li{ background:#CCC; }
- 给列表加MOUSEOVER背景变色效果：
   ul{ list-style-type: none; margin:0px; }
   ul li a{ display:block;  width: 100%; background:#ccc; }
   ul li a:hover{ background:#999; }
   //display:block;这一行必须要加的，这样才能块状显示！
- `<li>`中的元素水平排列,关键FLOAT:LEFT：
   ul{ list-style-type:none; width:100%; }
   ul li{ width:80px; float:left; }

#### ul ol dl分别在什么情况下用？

- 有序列表在列表项目对顺序有要求的时候使用；
- 无序列表在列表项目对顺序没要求时使用，可以是任何一系列项目；
- 定义列表在对项目有描述要求时使用。

