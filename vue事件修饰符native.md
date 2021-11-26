# Vue事件修饰符native和self

[Vue事件修饰符native和self](https://juejin.cn/post/6844903885916618759)

### native

`native`是原生事件

`native` 一定是用于自定义组件，也就是自定义的`html`标签

```
<body>
    <div id="app">
        <div class="box" >
            <Son @click='handler1'></Son>
        </div>
    </div>

</body>
<script>
    const Son = Vue.component('Son', {
        template: '<button @mouseover=handler2 class="box1">son</button>',
        methods: {
            handler2 (e) {
                
            }
        }
    })
    new Vue({
        el: "#app",
        components: {
            Son
        },
        data() {
            return {
                a: 123
            }
        },
        methods: {
            handler1 (e) {
                console.log('父级')
            }
        }
    })

</script>
复制代码
```

**注意点**

1. 当`<Son @click='handler1'></Son>`，子组件中的`this.$listeners`返回的是`{click: ƒ}`，**box1的dom上没有绑定click事件（可以打开F12查看）**，所以这个事件不是原生的`click`
2. 当`<Son @click.native='handler1'></Son>`，子组件中的`this.$listeners`返回的是`{}`，**box1的dom上绑定了click事件（可以打开F12查看）**，所以这个事件是原生的`click`
3. 当`<Son @click.self='handler1'></Son>`，子组件中的`this.$listeners`返回的是`{click: ƒ}`，**box1的dom上没有绑定click事件（可以打开F12查看）**，所以这个事件不是原生的`click`
4. 子组件的`this.$emit('eventTpye')`是从`this.$listeners`返回值中查找的

**为什么有时候组件点击事件不会生效** 猜测

1. 子组件`html`标签没有定义`click`原生事件
2. 子组件没有执行`this.$emit('click')`

所以

1. 直接`.native`将事件绑定到子组件`html`标签上，类似`dom.addEventListener('click', handler)`

### self

#### 作用

引用官方说明

```
<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
复制代码
```

结合代码说明

```
<body>
    <div id="app">
        <div class="box" @click.self='handler1'>
            <Son ></Son>
        </div>
    </div>

</body>
<script>
    const Son = Vue.component('Son', {
        template: '<button @click=handler2 class="box1">son</button>',
        methods: {
            handler2 (e) {
                console.log(e.target, e.currentTarget)
            }
        }
    })
    new Vue({
        el: "#app",
        components: {
            Son
        },
        data() {
            return {
                a: 123
            }
        },
        methods: {
            handler1 (e) {
                console.log(e.target, e.currentTarget)
            }
        }
    })
</script>
复制代码
```

1. 就是利用`e.target和e.currentTarget`，当添加`self`时，只有当两者相等时才会触发回调


作者：arhebin
链接：https://juejin.cn/post/6844903885916618759
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。