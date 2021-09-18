前端项目通常会用`@/`来做为别名映射路径`src/`， 但是这样就没有办法在引用文件的时候通过`.`来唤起路径提示了。

vscode官方提供一个方法：[jsconfig.json](https://link.juejin.im/?target=https%3A%2F%2Fcode.visualstudio.com%2Fdocs%2Flanguages%2Fjsconfig)

很简单，在项目根目录下，添加一个jsconfig.json文件，内容如下

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    },
    "target": "ES6",
    "module": "commonjs",
    "allowSyntheticDefaultImports": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

这样，在引用js文件的时候就会照常提示了

