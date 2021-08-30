##### 在 Vue 项目开发过程中，遇到需要重新使用 data 中的数据，但是data中的数据已经被赋值改变了，如何重置data的值为初始状态呢? 

解决办法：

##### 1. 直接赋值（不建议使用，若有多个地方需要重新设置数据，产生大量重复冗余的代码）

```
this.form = { 需要设置的数据 }
```

##### 2. 使用this.$options.data()获取初始 data 的值，再使用Object.assign()来复制对象; 

this.$data：获取当前状态下的 data,也就是要改变的 data 数据;    

this.$options.data(): vue原始的数据，就是你页面刚加载时的 data

```
Object.assign(this.$data, this.$options.data())
```

![img](media/cb44f7a741b04b4a86d2c0a9a7b1a778tplv-k3u1fbpfcp-watermark.awebp)

##### 若需要给某一个具体的数据 (eg: form) 重新设置值，则使用如下的方法，eg:

```
Object.assign(this.$data.form, this.$options.data().form)
```

注意：**data()中若使用了 this 来访问 props 或 methods，用 this.$options.data() 重置组件data时，data()里用 this 获取的 props 或 method 都为 undefined，注意 this.$options.data() 的 this 指向，最好使用 this.$options.data.call(this)。**



作者：前端代码仔
链接：https://juejin.cn/post/6918249086856462343
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。