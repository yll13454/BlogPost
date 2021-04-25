# [浅谈Vue3的watchEffect用途](https://segmentfault.com/a/1190000023669309)

watch api 在一个属性变更的时候，去执行我们想要的行为。比如：

- 当ID改变的时候，从数据库里面获取新的数据。
- 当属性变换的时候执行一个动画。
- 当搜索条件变更的时候，更新查询到的数据。

vue3 新增了一个 watchEffect 的 api

```
// 例子灵感来源于[文档](https://v3.vuejs.org/api/computed-watch-api.html#watcheffect)

import { watchEffect, ref } from 'vue'
setup () {
    const userID = ref(0)
    watchEffect(() => console.log(userID))
    setTimeout(() => {
      userID.value = 1
    }, 1000)

    /*
      * LOG
      * 0 
      * 1
    */

    return {
      userID
    }
 }
```

## 与 watch 有什么不同？

- 第一点我们可以从示例代码中看到 `watchEffect` 不需要指定监听的属性，他会自动的收集依赖， 只要我们回调中引用到了 响应式的属性， 那么当这些属性变更的时候，这个回调都会执行，而 `watch` 只能监听指定的属性而做出变更(v3开始可以同时指定多个)。
- 第二点就是 watch 可以获取到新值与旧值（更新前的值），而 `watchEffect` 是拿不到的。
- 第三点是 watchEffect 如果存在的话，在组件初始化的时候就会执行一次用以收集依赖（与`computed`同理），而后收集到的依赖发生变化，这个回调才会再次执行，而 watch 不需要，因为他一开始就指定了依赖。

### 停止监听

atchEffect 会返回一个用于停止这个监听的函数，如法如下：

```
const stop = watchEffect(() => {
  /* ... */
})

// later
stop()
```

例子来源于官方文档。

如果 `watchEffect` 是在 `setup` 或者 生命周期里面注册的话，在组件取消挂载的时候会自动的停止掉。

### 使 side effect 无效

什么是 side effect ,不可预知的接口请求就是一个 side effect，假设我们现在用一个用户ID去查询用户的详情信息，然后我们监听了这个用户ID， 当用户ID 改变的时候我们就会去发起一次请求，这很简单，用watch 就可以做到。 但是如果在请求数据的过程中，我们的用户ID发生了多次变化，那么我们就会发起多次请求，而最后一次返回的数据将会覆盖掉我们之前返回的所有用户详情。这不仅会导致资源浪费，还无法保证 watch 回调执行的顺序。而使用 `watchEffect` 我们就可以做到。

#### `onInvalidate()`

`onInvalidate(fn)`传入的回调会在 `watchEffect` 重新运行或者 `watchEffect` 停止的时候执行

```
watchEffect(() => {
      // 异步api调用，返回一个操作对象
      const apiCall = someAsyncMethod(props.userID)

      onInvalidate(() => {
        // 取消异步api的调用。
        apiCall.cancel()
      })
})
```

