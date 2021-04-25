![在这里插入图片描述](D:\笔记\media\20191105195158272.png)

### merge

假设现在HEAD在（6.added hello.txt file）处，也就是在master分支最近的一次提交处，此时执行git merge develop, 结果如下图所示。
![在这里插入图片描述](D:\笔记\media\2019110519521430.png)
工作原理就是：git 会自动根据两个分支的共同祖先即 (3.added merge.txt file)这个 commit 和两个分支的最新提交即 (6.added hello.txt file) 和 (5.added test.txt file) 进行一个三方合并，然后将合并中修改的内容生成一个新的 commit，即图二的(7.Merge branch ‘develop’)。
这是merge的效果，简单来说就合并两个分支并生成一个新的提交。

### rebase

![在这里插入图片描述](D:\笔记\media\20191105195247664.png)

![在这里插入图片描述](D:\笔记\media\20191105195300453.png)

在执行git rebase develop之前，HEAD在（6.added hello.txt file）处，当执行rebase操作时，git 会从两个分支的共同祖先 (3.added merge.txt file)开始提取 当前分支（此时是master分支）上的修改，即 （6.added hello.txt file）这个commit，再将 master 分支指向 目标分支的最新提交（此时是develop分支）即（5.added test.txt file） 处，然后将刚刚提取的修改应用到这个最新提交后面。如果提取的修改有多个，那git将依次应用到最新的提交后面，如上两图所示，图四为初始状态，图五为执行rebase后的状态。

简单来说，git rebase提取操作有点像git cherry-pick一样，执行rebase后依次将当前的提交cherry-pick到目标分支上，然后将在原始分支上的已提交的commit删除。

##### merge OR rebase

1. 可以看出merge结果能够体现出时间线，但是rebase会打乱时间线。
2. 而rebase看起来简洁，但是merge看起来不太简洁。
3. 最终结果是都把代码合起来了，所以具体怎么使用这两个命令看项目需要。

还有一点说明的是，在项目中经常使用git pull来拉取代码，git pull相当于是git fetch + git merge，如果此时运行git pull -r，也就是git pull --rebase，相当于git fetch + git rebase