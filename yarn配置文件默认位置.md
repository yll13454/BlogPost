# 项目场景：

在win10中，使用命令（目录填写错误）修改全局安装目录后

```
	yarn config set global-folder  ”D:\Program Files\Yarn\Cache“"
1
```

yarn报错error An unexpected error occurred: “EINVAL: invalid argument, mkdir 'D:\Program Files\Yarn\Cache”’"

------

# 问题描述：

设置错误的全局安装目录后，且目录未创建，目录中包含windows禁止的字符，如上述中使用到的“双引号”，无法正常使用yarn命令。

```
PS D:\IDEA-workspace\display> yarn config list
yarn config v1.22.5
error An unexpected error occurred: "EINVAL: invalid argument, mkdir 'D:\\Program Files\\Yarn\\Cache\"'".
info If you think this is a bug, please open a bug report with the information provided in "D:\\IDEA-workspace\\display\\yarn-error.log".
info Visit https://yarnpkg.com/en/docs/cli/config for documentation about this command.
12345
```



------

# 原因分析：

全局目录填写错误后，目录未创建，目录中包含windows禁止非法字符，使用yarn命令，yarn会先创建目录，由于目录创建失败，导致命令无法正确执行。

------

# 解决方案：

去c盘的对应用户的文件夹下找到**.yarnrc**文件。修改文件中的内容即可。详情见下列
C:\Users\你的用户名

![.yarnrc文件](https://img-blog.csdnimg.cn/20200924112537299.png#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200924112818578.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0RhcmtfUmVwdWxzZXI=,size_16,color_FFFFFF,t_70#pic_center)