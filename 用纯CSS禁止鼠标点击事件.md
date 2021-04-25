# 用纯CSS禁止鼠标点击事件

JavaScript有一个preventDefault方法, 他可用以来取消事件的默认动作。比如取消打开链接，选择文本或拖放等。

```css
event.preventDefault()
```

这种方法可以阻止当前元素的浏览器默认行为，但并不阻止事件被父级及document响应。如果想彻底取消事件，则可使用stopPropagation

```js
event.stopPropagation()
```

该方法将停止事件的传播，阻止它被分派到其他 Document 节点。在事件传播的任何阶段都可以调用它。注意，虽然该方法不能阻止同一个 Document 节点上的其他事件句柄被调用，但是它可以阻止把事件分派到其他节点。

### 用纯css就能实现取消事件响应的方法pointer-events

它可以：

1 阻止用户的点击动作产生任何效果

2 阻止缺省鼠标指针的显示

3 阻止CSS里的hover和active状态的变化触发事件

4 阻止JavaScript点击动作触发的事件

```css
.disabled {
    pointer-events: none;
    cursor: default;
    opacity: 0.6;
}
```

`pointer-events`的事项：

- 子元素可以声明`pointer-events`来解禁父元素的阻止鼠标事件限制。
- 如果你对一个元素设置了click事件监听器，然后你移除了`pointer-events`样式声明，或把它的值改变为`auto`，监听器会重新生效。基本上，监听器会遵守`pointer-events`的设定。