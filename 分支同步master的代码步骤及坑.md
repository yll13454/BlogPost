1、将分支代码feature-v3同步到master上

```
git checkout feature-v3
git add .
git commit -m "..."
git checkout master
git merge feature-v3
// 处理冲突 git add & git commit
git push1234567
```

2、将master代码同步到feature-v3上

```
git checkout master
git pull
git checkout feature-v3
git merge master
// 处理冲突 git add & git commit 
git push123456
```

3、注意点：

- 本地merge完了之后一定要push**对应的分支**，否则远程仓库并未同步！
- 若暂时没有被CR，那么远程仍是未同步的，当未同步时再同步其他的分支时，会有些diff的，因为就相当于未push成功嘛～
- 最后，再重复下，一定要push对应的分支！

