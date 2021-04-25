### watch

```js
class Watcher{
  constructor(vm,exprOrFn,callback,options,isRenderWatcher){
  }
}
```

- `vm` vue实例
- `exprOrFn` 可能是字符串或者回调函数(有点懵就往后看，现在它不重要)
- `options` 各种配置项(配置啥，往后看)
- `isRenderWatcher` 是否是**渲染`Wathcer`**

### initState