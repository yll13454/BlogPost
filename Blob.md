### 一、Blob 是什么

Blob（Binary Large Object）表示二进制类型的大对象。在数据库管理系统中，将二进制数据存储为一个单一个体的集合。Blob 通常是影像、声音或多媒体文件。**在 JavaScript 中 Blob 类型的对象表示不可变的类似文件对象的原始数据。** 为了更直观的感受 Blob 对象，我们先来使用 Blob 构造函数，创建一个 myBlob 对象，具体如下图所示：

![img](https://mmbiz.qpic.cn/mmbiz_jpg/jQmwTIFl1V3U8yD1AuibqbnUoI5LDmUacvkRpdO2GBxF3iaZibVj9Al2sTPmJvbryVC3DB3Vy9pbj61icgVZArfqgA/640?wx_fmt=jpeg)

`size` 属性用于表示数据的大小（以字节为单位），`type` 是 MIME 类型的字符串。Blob 表示的不一定是 JavaScript 原生格式的数据。比如 `File` 接口基于 `Blob`，继承了 blob 的功能并将其扩展使其支持用户系统上的文件。

### 二、Blob API 简介

`Blob` 由一个可选的字符串 `type`（通常是 MIME 类型）和 `blobParts` 组成：

![img](https://mmbiz.qpic.cn/mmbiz_jpg/jQmwTIFl1V3U8yD1AuibqbnUoI5LDmUaczscA8mdjFicU2ZibQDfTQrFL8Wy0bedjLhm96aRmEkGGDwolmk1Boo5A/640?wx_fmt=jpeg)

>MIME（Multipurpose Internet Mail Extensions）多用途互联网邮件扩展类型，是设定某种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。

常见的 MIME 类型有：超文本标记语言文本 .html text/html、PNG图像 .png image/png、普通文本 .txt text/plain 等。