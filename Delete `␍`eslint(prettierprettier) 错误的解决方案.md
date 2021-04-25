我的项目仓库中默认是`Linux`环境下提交的代码，文件默认是以`LF`结尾的(工程化需要，统一标准)。

当我用`windows`电脑`git clone`代码的时候，若我的`autocrlf`(在`windows`下安装`git`，该选项默认为`true`)为`true`，那么文件每行会被自动转成以`CRLF`结尾，若对文件不做任何修改，`pre-commit`执行`eslint`的时候就会提示你删除`CR`。

现在可以理解`ctrl+s`和`yarn run lint --fix`方案为何可以修复`eslint`错误了吧，因为`Git`自动将`CRLF`转换成了`LF`。


作者：羽徵
链接：https://juejin.cn/post/6844904069304156168
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

**最佳实践：**

现在`VScode`，`Notepad++`编辑器都能够自动识别文件的换行符是`LF`还是`CRLF`。 如果你用的是`windows`，文件编码是`UTF-8`且包含中文，最好全局将`autocrlf`设置为`false`。

```
git config --global core.autocrlf false

复制代码
```

注意：`git`全局配置之后，你需要重新拉取代码。

The `.prettierrc` file:

"endOfLine":"crlf"





