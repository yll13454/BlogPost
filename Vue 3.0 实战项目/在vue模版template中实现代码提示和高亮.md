### 解决方案

前提是：需要安装 `Vetur` 插件，就是这个货：

![《VSCode 中 Vue 的 Template 高亮提示》](D:\笔记\Vue 3.0 实战项目\media\image-1595991337916.png)

![在这里插入图片描述](D:\笔记\Vue 3.0 实战项目\media\20201110215931523.png)

![在这里插入图片描述](D:\笔记\Vue 3.0 实战项目\media\20201110220003269.png)

然后增加一条语句：

![在这里插入图片描述](D:\笔记\Vue 3.0 实战项目\media\20201110220122992.png)

```
"vetur.experimental.templateInterpolationService": true,
```

然后在 vue 文件中的 template 中可以自动识别 ts 语法定义的属性了

![在这里插入图片描述](D:\笔记\Vue 3.0 实战项目\media\20201110220311805.png)