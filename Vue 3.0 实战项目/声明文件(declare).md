## 什么是声明文件?

**声明文件就是给js代码补充类型标注**. 这样在ts编译环境下就不会提示js文件"缺少类型".

声明变量使用关键字`declare`来表示声明其后面的**全局**变量的类型, 比如:

```js
// packages/global.d.ts
declare var __DEV__: boolean
declare var __TEST__: boolean
declare var __BROWSER__: boolean
declare var __RUNTIME_COMPILE__: boolean
declare var __COMMIT__: string
declare var __VERSION__: string
```

看过vue3源码的同学一定知道这些是vue中的变量, 上面代码表示`__DEV__`等变量是全局, 并且标注了他们的类型. 这样无论在项目中的哪个ts文件中使用`__DEV__`, 变量ts编译器都会知道他是`boolean`类型.

声明文件在哪里?
首先声明文件的文件名是有规范要求的, 必须以**.d.ts**结尾, 看了上面的代码你肯定想练习写下声明文件, 但是你可能想问了"写完放在哪里", 网上说声明文件放在项目里的任意路径/文件名都可以被ts编译器识别, 但实际开发中发现, 为了规避一些奇怪的问题, 推荐放在根目录下.
![f0458427a788415372f00e48742f5b98.png](.\media\f0458427a788415372f00e48742f5b98.png) 

别人写好的声明文件( @types/xxx )
一般比较大牌的第三方js插件在npm上都有对应的声明文件, 比如jquery的声明文件就可以在npm上下载:

> npm i @types/jquery

npm i @types/jquery中的jquery可以换成任意js库的名字, 当然前提是有人写了对应的声明文件发布到了npm.

安装后, 我们可以在node_modules/@types/jquery中的看到声明文件, 这里我打开mise.d.ts文件:
![9b410fa0a0d0af7afd768970f92d99f4.png](D:\笔记\Vue 3.0 实战项目\media\9b410fa0a0d0af7afd768970f92d99f4.png)

## 声明文件对纯js项目有什么帮助?

即便你只写js代码, 也可以安装声明文件, 因为如果你用的是**vscode**, 那么他会自动分析js代码, 如果存在对应的声明文件, vscode会把声明文件的内容作为**代码提示**.

### declare namespace

这个namespace代表后面的全局变量是一个对象:

```js
// global.d.ts
declare namespace MyPlugin {
    var n:number;
    var s:string;
    var f:(s:string)=>number;
}
MyPlugin.s.substr(0,1);
MyPlugin.n.toFixed();
MyPlugin.f('文字').toFixed();

// 报错
MyPlugin.s.toFixed();
MyPlugin.n.substr(0,1);
MyPlugin.f(123);
```

### 修改已存在的全局声明

如果你要修改已存在的全局变量的声明可以这么写, 下面用node下的global举例,

```js
declare global {
    interface String {
        hump(input: string): string;
    }
}
// 注意: 修改"全局声明"必须在模块内部, 所以至少要有 export{}字样
// 不然会报错❌: 全局范围的扩大仅可直接嵌套在外部模块中或环境模块声明中
export {}
```

现在String类型在vscode的语法提示下多了一个hump的方法,不过我们只是声明, 并没有用js实现, 所以运行会报错, 所以不要忘了写js的实现部分哦.
————————————————
版权声明：本文为CSDN博主「kitenancy」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_35390057/article/details/112494096