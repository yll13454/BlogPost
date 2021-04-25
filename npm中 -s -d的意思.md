

>npm install moduleName # 安装模块到项目目录下
>
>npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
>
>npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
>
>npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。

## devDependencies和dependencies 节点

> 对于我们依赖的这些插件库，有的是我们开发所使用的，有的则是项目所依赖的。对于这个分界线，我们诞生了`**dependencies**`和`**devDependencies**`

**devDependencies 开发环境使用**

比如项目中使用的 gulp ，压缩css、js的模块。这些模块在我们的项目部署后是不需要的，所以我们可以使用 -save-dev 的形式安装。

**dependencies 生产环境使用**

像 `express` `jquery`这些模块是项目运行必备的。

- **npm install moduleName 命令**

1. 安装模块到项目node_modules目录下。
2. 不会将模块依赖写入devDependencies或dependencies 节点。
3. 运行 npm install 初始化项目时不会下载模块。

- **npm install -g moduleName 命令**

1. 安装模块到全局，不会在项目node_modules目录中保存模块包。
2. 不会将模块依赖写入devDependencies或dependencies 节点。
3. 运行 npm install 初始化项目时不会下载模块。

- **npm install --save moduleName 命令**

1. 安装模块到项目node_modules目录下。
2. 会将模块依赖写入dependencies 节点。
3. 运行 npm install 初始化项目时，会将模块下载到项目目录下。
4. 运行npm install --production或者注明NODE_ENV变量值为production时，会自动下载模块到node_modules目录中。

- **npm install --save-dev moduleName 命令**

1. 安装模块到项目node_modules目录下。
2. 会将模块依赖写入devDependencies 节点。
3. 运行 npm install 初始化项目时，会将模块下载到项目目录下。
4. 运行npm install --production或者注明NODE_ENV变量值为production时，不会自动下载模块到node_modules目录中。

- **简写
  **npm install module_name -S 即 npm install module_name --save

npm install module_name -D 即 npm install module_name --save-dev



#### devDependencies  里面的插件只用于开发环境，不用于生产环境，而 dependencies  是需要发布到生产环境的。