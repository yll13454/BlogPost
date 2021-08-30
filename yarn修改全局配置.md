# [Yarn的安装和全局配置(源/缓存位置/全局安装位置)](https://www.cnblogs.com/hellomrr/p/13237653.html)

> 本文安装环境: Win10 64位
> 前置条件: 已安装好Node环境(参考[Node安装与环境配置](https://blog.csdn.net/VXadmin/article/details/89332672))

## 下载和安装

[Yarn安装包下载地址](https://classic.yarnpkg.com/zh-Hans/docs/install#windows-stable)

## 全局配置

控制台输入命令, 正常显示版本表示安装成功

```powershell
$ yarn -v		# 查看yarn版本
```

查看yarn的所有配置

```powershell
$ yarn config list		# 查看yarn配置
```

修改yarn的源镜像为淘宝源

```powershell
$ yarn config set registry https://registry.npm.taobao.org/
```

修改全局安装目录, 先创建好目录(global), 我放在了Yarn安装目录下(D:\RTE\Yarn\global)

```powershell
$ yarn config set global-folder "D:\RTE\Yarn\global"		# 具体目录请改成自己的
```

修改全局安装目录的bin目录位置, bin目录需要自己创建, 而且需要把此目录加到系统环境变量(D:\RTE\Yarn\global\bin), 添加环境变量请参考: [环境变量](https://blog.csdn.net/VXadmin/article/details/89399422)

```powershell
$ yarn config set prefix "D:\RTE\Yarn\global\"		# 会自动设置成*\global\bin 
```

修改全局缓存目录, 先创建好目录(cache), 和global放在同一层目录下

```powershell
$ yarn config set cache-folder "D:\RTE\Yarn\cache"			# 具体目录请改成自己的
```

查看所有配置

```powershell
yarn config list
```

查看当前yarn的bin的位置

```powershell
$ yarn global bin
```

查看当前yarn的全局安装位置

```powershell
$ yarn global dir
```