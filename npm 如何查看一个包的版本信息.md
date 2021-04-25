第一种方式：使用npm view jquery versions

​          这种方式可以查看npm服务器上所有的jquery版本信息；

​          ![img](D:\笔记\media\20180313173055768)

第二种方式：使用npm view jquery version

​          这种方式只能查看jquery的最新的版本是哪一个；

​          ![img](D:\笔记\media\20180313173103644)

第三种方式：使用npm info jquery

​          这种方式和第一种类似，也可以查看jquery所有的版本，

​          但是能查出更多的关于jquery的信息；

​          ![img](D:\笔记\media\20180313173112300)





假设现在我们已经成功下载了jquery，过了一段时间，我忘记了下载的jquery的版本信息，

这个时候，我们就需要查看本地下载的jquery版本信息，怎么做呢？

第一种方式：npm ls jquery 即可（查看本地安装的jQuery），下面我的本地没有安装jquery，

​          所以返回empty的结果；

​          ![img](D:\笔记\media\20180313173527357)

第二种方式：npm ls jquery -g  (查看全局安装的jquery)

​          ![img](D:\笔记\media\20180313173534437)

​         

总结：上面我们了解了如何通过npm 来查看我们需要的包的版本信息，

​      既可以查看远端npm 服务器上的，也可以查看本地的；