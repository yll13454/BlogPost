【Vue进阶】手把手教你在 Vue 中使用 JSX

https://www.jianshu.com/p/9616745a8d0a

渲染函数 & JSX

https://cn.vuejs.org/v2/guide/render-function.html

详解利用jsx写vue组件

https://www.jb51.net/article/118865.htm

## 基础内容

这里展示在 `Vue` 中书写一些基础内容，包括纯文本、动态内容、标签使用、自定义组件的使用，这些跟我们平时使用单文件组件类似，如下所示：

```js
render() {
  return (
    <div>
      <h3>内容</h3>
      {/* 纯文本 */}
      <p>hello, I am Gopal</p>
      {/* 动态内容 */}
      <p>hello { this.msg }</p>
      {/* 输入框 */}
      <input />
      {/* 自定义组件 */}
      <myComponent></myComponent>
    </div>
  );
}
```

## Attributes/Props

`Attributes` 的绑定跟普通的 `HTML` 结构一样

```js
render() {
  return <div><input placeholder="111" /></div>
}
```

注意，如果动态属性，之前是 `v-bind:placeholder="this.placeholderText"` 变成了`placeholder={this.placeholderText}`

```kotlin
render() {
  return <input
            type="email"
            placeholder={this.placeholderText}
          />
}
```

我们也可以展开一个对象

```js
render (createElement) {
    return (
        <button {...this.largeProps}></button>
    )
}
```

像 `input` 标签，就可以如下批量绑定属性

```go
const inputAttrs = {
    type: 'email',
    placeholder: 'Enter your email'
};
render() {
  return <input {...{ attrs: inputAttrs }} /> 
}
```



作者：Gopal1
链接：https://www.jianshu.com/p/9616745a8d0a
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。