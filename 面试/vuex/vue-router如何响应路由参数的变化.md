### 什么是路由参数的变化

当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。

### 监测路由参数变化的方法（watch监听|导航守卫）

**方法一watch监听：**



> ```
> watch: { // watch的第一种写法
> ```
>
> **`$route (to, from) {`**
>
> **`console.log(to)`**
>
> **`console.log(from)`**
>
> ```
> }
> },
> watch: { // watch的第二种写法
> ```
>
> **`$route: {`**
>
> **`handler (to, from){`**
>
> **`console.log(to)`**
>
> **`console.log(from)`**
>
> **`},`**
>
> ```
> // 深度观察监听
> ```
>
> **`deep: true`**
>
> ```
> }
> },
> watch: { // watch的第三种写法
> ```
>
> **`'$route':'getPath'`**
>
> ```
> },
> methods: {
> ```
>
> **`getPath(to, from){`**
>
> **`console.log(this.$route.path);`**
>
> **`}`**
>
> ```
> },
> ----------------------------------------------------------------
> 举例：
> ```
>
> watch: {
>
> // 方法1 //监听路由是否变化
>
> '$route' (to, from) {
>
> if(to.query.id !== from.query.id){
>
> this.id = to.query.id;
>
> this.init();//重新加载数据
>
> }
>
> }
>
> }
>
> //方法 2 设置路径变化时的处理函数
>
> watch: {
>
> '$route': {
>
> handler: 'init',
>
> immediate: true
>
> }
>
> 为了实现这样的效果**可以给`router-view`添加一个不同的`key`**，这样即使是公用组件，只要url变化了，就一定会重新创建这个组件。
>
> ```
> <router-view :key="$route.fullpath"></router-view>
> ```

**方法二：导航守卫**

> **`beforeRouteEnter (to, from, next) {`**
>
> ```
> console.log('beforeRouteEnter被调用：在渲染该组件的对应路由被 confirm 前调用')
> // 在渲染该组件的对应路由被 confirm 前调用
> // 不！能！获取组件实例 `this` 因为当守卫执行前，组件实例还没被创建
> // 可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
> next(vm => {
> // 通过 `vm` 访问组件实例
> console.log(vm)
> })
> ```
>
> **`},`**
>
> ```
> // beforeRouteEnter 是支持给 next 传递回调的唯一守卫。
> // 对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。
> ```
>
> **`beforeRouteUpdate (to, from, next) {`**
>
> ```
> // 在当前路由改变，但是该组件被复用时调用
> // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
> // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
> ```
>
> `// 可以访问组件实例 `this``
>
> ```
> console.log('beforeRouteUpdate被调用：在当前路由改变，但是该组件被复用时调用')
> next()
> },
> ```
>
> **`beforeRouteLeave (to, from, next) {`**
>
> ```
> // 导航离开该组件的对应路由时调用
> ```
>
> `// 可以访问组件实例 `this``
>
> ```
> const answer = window.confirm('是否确认离开当前页面')
> if (answer) {
> console.log('beforeRouteLeave被调用：导航离开该组件的对应路由时调用')
> next()
> } else {
> next(false)
> }
> ```
>
> **`},`**
>
>  