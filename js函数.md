## addEventListener

就这，就这，还怀疑我不会用。 我就真觉得，看到这里的有50%以上，就真不全会。 等看完，如果是真不全会的，麻烦评论区评论 `一起围观作者`。 ;

```js
target.addEventListener(type, listener, options);
target.addEventListener(type, listener, useCapture)
复制代码
```

四个9的可靠性，`99.9999%`的通知，都完全知道第三个参数是布尔值的情况。我也就不说了。

**options 可选**

可用的选项：

- capture:  Boolean，表示 listener 会在该类型的事件捕获阶段传播到该 EventTarget 时触发。
- once:  Boolean，表示 listener 在添加之后最多只调用一次。如果是 true， listener 会在其被调用之后自动移除。
- passive: Boolean，设置为true时，表示 listener 永远不会调用 preventDefault()。如果 listener 仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。查看 [使用 passive 改善的滚屏性能](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FEventTarget%2FaddEventListener%23%E4%BD%BF%E7%94%A8_passive_%E6%94%B9%E5%96%84%E7%9A%84%E6%BB%9A%E5%B1%8F%E6%80%A7%E8%83%BD) 了解更多.
- signal：AbortSignal，该 AbortSignal 的 abort() 方法被调用时，监听器会被移除。




### passive

查看 [使用 passive 改善的滚屏性能](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FAPI%2FEventTarget%2FaddEventListener%23%E4%BD%BF%E7%94%A8_passive_%E6%94%B9%E5%96%84%E7%9A%84%E6%BB%9A%E5%B1%8F%E6%80%A7%E8%83%BD) 了解更多