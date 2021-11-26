## 什么是 pnpm ?

pnpm 本质上就是一个包管理器，这一点跟 npm/yarn 没有区别，但它作为杀手锏的两个优势在于:

- 包安装速度极快；
- 磁盘空间利用非常高效。

```
npm i -g pnpm
```

## 特性概览

### 1. 速度快

yarn 不是有 [PnP 安装模式](https://link.juejin.cn/?target=https%3A%2F%2Fclassic.yarnpkg.com%2Fen%2Fdocs%2Fpnp%2F)吗？直接去掉 node_modules，将依赖包内容写在磁盘，节省了 node 文件 I/O 的开销，这样也能提升安装速度。（具体原理见[这篇文章](https://link.juejin.cn/?target=https%3A%2F%2Floveky.github.io%2F2019%2F02%2F11%2Fyarn-pnp%2F)）

### 2. 高效利用磁盘空间

pnpm 内部使用`基于内容寻址`的文件系统来存储磁盘上所有的文件，这个文件系统出色的地方在于:

- 不会重复安装同一个包。用 npm/yarn 的时候，如果 100 个项目都依赖 lodash，那么 lodash 很可能就被安装了 100 次，磁盘中就有 100 个地方写入了这部分代码。但在使用 pnpm 只会安装一次，磁盘中只有一个地方写入，后面再次使用都会直接使用 `hardlink`(硬链接，不清楚的同学详见[这篇文章](https://link.juejin.cn?target=https%3A%2F%2Fwww.cnblogs.com%2Fitech%2Farchive%2F2009%2F04%2F10%2F1433052.html))。
- 即使一个包的不同版本，pnpm 也会极大程度地复用之前版本的代码。举个例子，比如 lodash 有 100 个文件，更新版本之后多了一个文件，那么磁盘当中并不会重新写入 101 个文件，而是保留原来的 100 个文件的 `hardlink`，仅仅写入那`一个新增的文件`。

### 3. 支持 monorepo

随着前端工程的日益复杂，越来越多的项目开始使用 monorepo。之前对于多个项目的管理，我们一般都是使用多个 git 仓库，但 monorepo 的宗旨就是用一个 git 仓库来管理多个子项目，所有的子项目都存放在根目录的`packages`目录下，那么一个子项目就代表一个`package`。如果你之前没接触过 monorepo 的概念，建议仔细看看[这篇文章](https://link.juejin.cn?target=https%3A%2F%2Fwww.perforce.com%2Fblog%2Fvcs%2Fwhat-monorepo)以及开源的 monorepo 管理工具[lerna](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Flerna%2Flerna%23readme)，项目目录结构可以参考一下 [babel 仓库](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fbabel%2Fbabel)。

pnpm 与 npm/yarn 另外一个很大的不同就是支持了 monorepo，体现在各个子命令的功能上，比如在根目录下 `pnpm add A -r`, 那么所有的 package 中都会被添加 A 这个依赖，当然也支持 `--filter`字段来对 package 进行过滤。

### 4. 安全性高

之前在使用 npm/yarn 的时候，由于 node_module 的扁平结构，如果 A 依赖 B， B 依赖 C，那么 A 当中是可以直接使用 C 的，但问题是 A 当中并没有声明 C 这个依赖。因此会出现这种非法访问的情况。但 pnpm 脑洞特别大，自创了一套依赖管理方式，很好地解决了这个问题，保证了安全性，具体怎么体现`安全`、规避非法访问依赖的`风险`的，后面再来详细说说。

## 三、依赖管理

### npm/yarn install 原理

主要分为两个部分, 首先，执行 npm/yarn install之后，`包如何到达项目 node_modules 当中`。其次，node_modules `内部如何管理依赖`。

执行命令后，首先会构建依赖树，然后针对每个节点下的包，会经历下面四个步骤:

- 1. 将依赖包的版本区间解析为某个具体的版本号
- 1. 下载对应版本依赖的 tar 包到本地离线镜像
- 1. 将依赖从离线镜像解压到本地缓存
- 1. 将依赖从缓存拷贝到当前目录的 node_modules 目录

然后，对应的包就会到达项目的`node_modules`当中。

**依赖在`node_modules`内部的目录结构**

在 `npm1`、`npm2` 中呈现出的是嵌套结构，比如下面这样:

```js
node_modules
└─ foo
   ├─ index.js
   ├─ package.json
   └─ node_modules
      └─ bar
         ├─ index.js
         └─ package.json
```

如果 `bar` 当中又有依赖，那么又会继续嵌套下去。

存在什么问题:

- 依赖层级太深，会导致文件路径过长的问题，尤其在 window 系统下。

- 大量重复的包被安装，文件体积超级大。比如跟 `foo` 同级目录下有一个`baz`，两者都依赖于同一个版本的`lodash`，那么 lodash 会分别在两者的 node_modules 中被安装，也就是重复安装。

- 模块实例不能共享。比如 React 有一些内部变量，在两个不同包引入的 React 不是同一个模块实例，因此无法共享内部变量，导致一些不可预知的 bug


接着，从 npm3 开始，包括 yarn，都着手来通过`扁平化依赖`的方式来解决这个问题。

相比之前的`嵌套结构`，现在的目录结构类似下面这样:

```js
node_modules
├─ foo
|  ├─ index.js
|  └─ package.json
└─ bar
   ├─ index.js
   └─ package.json
```

所有的依赖都被拍平到`node_modules`目录下，不再有很深层次的嵌套关系。这样在安装新的包时，根据 node require 机制，会不停往上级的`node_modules`当中去找，如果找到相同版本的包就不会重新安装，解决了大量包重复安装的问题，而且依赖层级也不会太深。

它照样存在诸多问题，梳理一下:

- 1. 依赖结构的**不确定性**。
- 1. 扁平化算法本身的**复杂性**很高，耗时较长。
- 1. 项目中仍然可以**非法访问**没有声明过依赖的包

后面两个都好理解，那第一点中的`不确定性`是什么意思？这里来详细解释一下。

假如现在项目依赖两个包 foo 和 bar，这两个包的依赖又是这样的: ![img](media/211c86c7838b48bead2a4ee7faee25b1tplv-k3u1fbpfcp-watermark.awebp)

那么 npm/yarn install 的时候，通过扁平化处理之后，究竟是这样 ![img](media/b2368d74f8b341f0b1b545198683af59tplv-k3u1fbpfcp-watermark.awebp)

还是这样？

![img](media/198d6c12f0e045ce8c0867b44b3aaeb1tplv-k3u1fbpfcp-watermark.awebp)

答案是: 都有可能。取决于 foo 和 bar 在 `package.json`中的位置，如果 foo 声明在前面，那么就是前面的结构，否则是后面的结构。

这就是为什么会产生依赖结构的`不确定`问题，也是 `lock 文件`诞生的原因，无论是`package-lock.json`(npm 5.x才出现)还是`yarn.lock`，都是为了保证 install 之后都产生确定的`node_modules`结构。

尽管如此，npm/yarn 本身还是存在`扁平化算法复杂`和`package 非法访问`的问题，影响性能和安全。

### pnpm 依赖管理

以安装 `express` 为例，我们新建一个目录，执行:

```
pnpm init -y
```

然后执行:

```
pnpm install express
```

我们再去看看`node_modules`:

```
.pnpm
.modules.yaml
express
```

我们直接就看到了`express`，但值得注意的是，这里仅仅只是一个`软链接`，不信你打开看看，里面并没有 node_modules 目录，如果是真正的文件位置，那么根据 node 的包加载机制，它是找不到依赖的。那么它真正的位置在哪呢？

我们继续在 .pnpm 当中寻找:

```
▾ node_modules
  ▾ .pnpm
    ▸ accepts@1.3.7
    ▸ array-flatten@1.1.1
    ...
    ▾ express@4.17.1
      ▾ node_modules
        ▸ accepts
        ▸ array-flatten
        ▸ body-parser
        ▸ content-disposition
        ...
        ▸ etag
        ▾ express
          ▸ lib
            History.md
            index.js
            LICENSE
            package.json
            Readme.md
```

`<package-name>@version/node_modules/<package-name>`这种目录结构。并且 express 的依赖都在`.pnpm/express@4.17.1/node_modules`下面，这些依赖也全都是**软链接**。

将`包本身`和`依赖`放在同一个`node_module`下面，与原生 Node 完全兼容，又能将 package 与相关的依赖很好地组织到一起，设计十分精妙。

## 五、日常使用

### pnpm install

跟 npm install 类似，安装项目下所有的依赖。可以通过 --filter 参数来指定 package，只对满足条件的 package 进行依赖安装。

```js
// 安装 axios
pnpm install axios
// 安装 axios 并将 axios 添加至 devDependencies
pnpm install axios -D
// 安装 axios 并将 axios 添加至 dependencies
pnpm install axios -S
```

当然，也可以通过 --filter 来指定 package。

### pnpm update

根据指定的范围将包更新到最新版本，monorepo 项目中可以通过 --filter 来指定 package。

### pnpm uninstall

在 node_modules 和 package.json 中移除指定的依赖。monorepo 项目同上。举例如下:

```
// 移除 axios
pnpm uninstall axios --filter package-a
```

### pnpm link

将本地项目连接到另一个项目。注意，使用的是硬链接，而不是软链接。如:

```
pnpm link ../../axios
```

