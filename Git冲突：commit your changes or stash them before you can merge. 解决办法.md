##### 用git pull来更新代码的时候，遇到了下面的问题：

```css
error: Your local changes to the following files would be overwritten by merge:
        vue.config.js
Please commit your changes or stash them before you merge.
```

出现这个问题的原因是其他人修改了xxx.php并提交到版本库中去了，而你本地也修改了xxx.php，这时候你进行git pull操作就好出现冲突了，解决方法，在上面的提示中也说的很明确了。

##### 1、保留本地的修改 的改法

```undefined
git stash
git pull
git stash pop
```

通过git stash将工作区恢复到上次提交的内容，同时备份本地所做的修改，之后就可以正常git pull了，git pull完成后，执行git stash pop将之前本地做的修改应用到当前工作区。

>git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。
>
>git stash pop: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复。
>
>git stash list: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。
>
>git stash clear: 清空Git栈。此时使用gitg等图形化工具会发现，原来stash的哪些节点都消失了。

##### 2、放弃本地修改 的改法 

```undefined
git reset --hard
git pull
```

# git 拉取仓库指定目录

```csharp
// 在本地指定文件夹内执行此命令设置为git仓库
git init 
// 拉取remote all objects信息
// 添加远程仓库地址，实现拉取remote的all objects信息
git remote add origin http://github/xxx.git 
// 开启sparse clone
// 用于控制是否允许设置pull指定文件/夹，适用于Git1.7.0以后版本，本质是开启sparse clone
git config core.sparsecheckout true
// 本地目录的.git文件夹下，如果没有sparse-checkout文件则创建，在其中添加指定的文件/夹fileName，就是需要拉取的那个特定文件/夹。*表示所有，！表示匹配相反
echo "fileName" >> .git/info/sparse-checkout 
// 查看
cat .git/info/sparse-chechout
// 拉取指定目录
// 拉取命令是一样的，只是已经通过配置文件sparse-chechout指定了目标文件/夹
git pull origin master 
```



作者：吉他手_c156
链接：https://www.jianshu.com/p/1a6e929ef405
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。