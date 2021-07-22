1.现象

![在这里插入图片描述](media/20200411094033475.png) 


2. 原因
img是一种类似text的元素，在结束的时候，会在末尾加上一个空白符，所以就会多出3px

3.解决办法
分两种：

设置img元素display:block 或者 vertical-align: middle;
设置div元素display:flex
————————————————
版权声明：本文为CSDN博主「选择远方」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_40713392/article/details/105447292

