# 前端错误监控方案 sentry

![img](media/b5471bcb57204cd6a82f1905aaa0353f~tplv-k3u1fbpfcp-zoom-1.image)  

更具体一点 **监控错误 -> 搜集错误 -> 存储错误 -> 分析错误 -> 错误报警-> 定位错误 -> 解决错误**

当错误发生的时候我们更需要一些辅助信息来帮我们更快的定位错误，比如 *JS错误类型、 JS错误信息、JS错误堆栈、JS错误发生的位置以及相关位置的代码；JS错误发生的几率、浏览器的类型，版本号，设备机型等等辅助信息*

## sentry

Sentry 是一个日志平台，分为客户端和服务端，客户端(目前客户端有Python, PHP,C#, Ruby等多种语言)就嵌入在你的应用程序中间，程序出现异常就向服务端发送消息，服务端将消息记录到数据库中并提供一个web节目方便查看。Sentry由python编写，源码开放，性能卓越，易于扩展，目前著名的用户有Disqus, Path, mozilla, Pinterest等

### 功能

<img src="media/4e09e0c1b67844c8be3799b6fbc1e985~tplv-k3u1fbpfcp-zoom-1.image" alt="img" style="zoom:80%;" /> 

### 上报过来的问题

这个就是你的应用所有的异常

1. 问题总数是指当前不同的问题，同一个问题出现的次数会在末尾那个数字体现
2. 然后管理员可以将这个问题勾选后分配给其它成员
3. 有的问题可以将其 ignore 或者 resolve, 之后就不会出现在你的列表里面
4. 还有自定义搜索： 最多出现、最后出现时间、首页出现等等

![img](media/e1e23da0d50a4d6c98f23c2b1df7b728~tplv-k3u1fbpfcp-zoom-1.image)

1. 当前错误所发生的 url
2. 浏览器的名字、版本、ua
3. 用户的设备信息例如 **XiaoMi MI MAX 3 android10.0 ipxxx**
4. 用户的基础信息例如 name 、userid等更多业务相关的需要自己配置
5. 如果页面有 xhr 信息会将请求信息响应状态码显示出来
6. 画重点——当碰到 js 错误的时候会将调用栈、错误类型、错误发生文章打印出来，如果配了**sourcemap** 可以将压缩混淆前的代码清楚的定位到哪一行

### 可视化

1. 内置各种维度的数据可视化
2. 如果不满意，它也提供了一些 api、类似 **gitlab、 github会提供一样的 api 进行二次开发**
   ![img](media/1837f2e8bc814c7c8af1830377e08424~tplv-k3u1fbpfcp-zoom-1.image)

### 报警

可以安装一些额外的插件，比如 **钉钉机器人、邮件提醒**

## 安装与部署 

### 安装方式

- Python
- Docker
   这里使用了 **docker** 安装比较简单一点

### 环境

```
MIN_DOCKER_VERSION='1.10.0' //docker -v
MIN_COMPOSE_VERSION='1.17.0' //docker-compose -v
MIN_RAM=3072 # MB //你的内存至少3G
```

### 一键生成

官方在19年的时候提供给了脚本一键生成的方式，仓库在这里onpremise[2]

```
git clone https://github.com/getsentry/onpremise
cd onpremise
./install.sh
```

如果 docker 没有配置国内镜像估计会很慢，提一下配置镜像

```
## 有的话就忽略
mkdir /etc/docker 
## 没有的直接执行
vim /etc/docker/daemon.json
{
 "registry-mirrors" : [
   "https://mirror.ccs.tencentyun.com",
   "http://registry.docker-cn.com",
   "http://docker.mirrors.ustc.edu.cn",
   "http://hub-mirror.c.163.com"
 ],
 ...
}
```

**中间会有一次让你配置管理员账号密码的过程** 下载完毕以后执行

```
docker-comose up -d
```

配置完以后基本上就整个安装过程应该不会有啥大问题，有问题估计是网络问题


作者：又在吃鱼
链接：https://juejin.cn/post/6875955097864994823
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

[vite-plugin-sentry](https://www.npmjs.com/package/vite-plugin-sentry)

https://juejin.cn/post/6854573216808861710

[前端错误监控系统 sentry 使用 webpack-sentry-plugin 插件上传 source-map 文件](https://bbs.huaweicloud.com/blogs/220124)