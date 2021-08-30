## reactive

### 定义

`reactive API` 的定义为传入一个对象并返回一个基于原对象的响应式代理，即返回一个 `Proxy`，相当于 `Vue2x` 版本中的 `Vue.observer`。

```js
  setup() {
    const state = reactive({
      count: 0,
      double: computed(() => state.count * 2)
    })

    function increment() {
      state.count++
    }

    return {
      state,
      increment
    }
  }
```

**使用**

```vue
<div>
	{{state.count}}
</div>
```

**注意：** `Proxy` 对象遇到解构和展开运算符后，会失去引用。

```js
   const foo = reactive({ bar: "" });
    const { buz } = foo;
    foo.bar = "lorem ipsum";
    console.log(foo.bar, buz); // => "lorem ipsum", ""
```

![img](media\171056493a9e69ae) 

可以看到，`toRefs` 是在原有 `Proxy` 对象的基础上，返回了一个普通的带有 `get` 和 `set` 的对象。这样就解决了 `Proxy` 对象遇到解构和展开运算符后，失去引用的情况的问题。

[When it's really needed to use `toRefs` in order to retain reactivity of `reactive` value](https://github.com/vuejs/rfcs/issues/145)