## vite 启动链路

### 命令解析

这部分代码在 [src/node/cli.ts](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Fvite%2Fblob%2Fmaster%2Fsrc%2Fnode%2Fcli.ts) 里，主要内容是借助 [minimist](https://link.juejin.cn?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fminimist) —— 一个轻量级的命令解析工具解析 npm scripts，解析的函数是 `resolveOptions` ，精简后的代码片段如下。

```
function resolveOptions() {
    // command 可以是 dev/build/optimize
    if (argv._[0]) {
        argv.command = argv._[0];
    }
    return argv;
}
复制代码
```

拿到 options 后，会根据 `options.command` 的值判断是执行在开发环境需要的 runServe 命令或生产环境需要的 runBuild 命令。

```
if (!options.command || options.command === 'serve') {
    runServe(options)
 } else if (options.command === 'build') {
    runBuild(options)
 } else if (options.command === 'optimize') {
    runOptimize(options)
 }
复制代码
```

在 `runServe` 方法中，执行 server 模块的创建开发服务器方法，同样在 `runBuild` 中执行 build 模块的构建方法。


作者：淘系前端团队
链接：https://juejin.cn/post/6844904176531537934
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。