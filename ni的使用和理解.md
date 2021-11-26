[尤雨溪推荐神器 ni ，能替代 npm/yarn/pnpm ？简单好用！源码揭秘！](https://juejin.cn/post/7023910122770399269)

[github 仓库 ni#how](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fantfu%2Fni%23how)

```
使用 `ni` 在项目中安装依赖时：
   假设你的项目中有锁文件 `yarn.lock`，那么它最终会执行 `yarn install` 命令。
   假设你的项目中有锁文件 `pnpm-lock.yaml`，那么它最终会执行 `pnpm i` 命令。
   假设你的项目中有锁文件 `package-lock.json`，那么它最终会执行 `npm i` 命令。

使用 `ni -g vue-cli` 安装全局依赖时
    默认使用 `npm i -g vue-cli`

当然不只有 `ni` 安装依赖。
    还有 `nr` - run
    `nx` - execute
    `nu` - upgrade
    `nci` - clean install
    `nrm` - remove
```

**我看源码发现：`ni`相关的命令，都可以在末尾追加`\?`，表示只打印，不是真正执行**。

![命令测试图示](media/bebec9efba69488ab2167ef1c6121781tplv-k3u1fbpfcp-watermark.awebp)

假设项目目录下没有锁文件，默认就会让用户从`npm、yarn、pnpm`选择，然后执行相应的命令。 但如果在`~/.nirc`文件中，设置了全局默认的配置，则使用默认配置执行对应命令。

**Config**

```ini
; ~/.nirc

; fallback when no lock found
defaultAgent=npm # default "prompt"

; for global installs
globalAgent=npm
复制代码
```

**因此，我们可以得知这个工具必然要做三件事**：

```bash
1. 根据锁文件猜测用哪个包管理器 npm/yarn/pnpm 
2. 抹平不同的包管理器的命令差异
3. 最终运行相应的脚本
```

## 3. 使用

[ni github文档](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fantfu%2Fni)。

全局安装。

```bash
npm i -g @antfu/ni
```

如果全局安装遭遇冲突，我们可以加上 `--force` 参数强制安装。

### 3.1 ni - install

```bash
ni

# npm install
# yarn install
# pnpm install

ni axios

# npm i axios
# yarn add axios
# pnpm i axios
```

### 3.2 nr - run

```bash
nr dev --port=3000

# npm run dev -- --port=3000
# yarn run dev --port=3000
# pnpm run dev -- --port=3000
nr
# 交互式选择命令去执行
# interactively select the script to run
# supports https://www.npmjs.com/package/npm-scripts-info convention
nr -

# 重新执行最后一次执行的命令
# rerun the last command
```

### 3.3 nx - execute

```bash
nx jest

# npx jest
# yarn dlx jest
# pnpm dlx jest
```

### 4.2 package.json 文件

```json
{
    "name": "@antfu/ni",
    "version": "0.10.0",
    "description": "Use the right package manager",
    // 暴露了六个命令
    "bin": {
        "ni": "bin/ni.js",
        "nci": "bin/nci.js",
        "nr": "bin/nr.js",
        "nu": "bin/nu.js",
        "nx": "bin/nx.js",
        "nrm": "bin/nrm.js"
    },
    "scripts": {
        // 省略了其他的命令 用 esno 执行 ts 文件
        // 可以加上 ? 便于调试，也可以不加
        // 或者是终端 npm run dev \?
        "dev": "esno src/ni.ts ?"
    },
}
```

根据 `dev` 命令，我们找到主入口文件 `src/ni.ts`。

yarn最初比npm多个lock，

pnpm又使用本地链接节省了硬盘和时间

