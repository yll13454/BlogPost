## `$attrs` 与 `$listeners`

> `$attrs`: 当组件在调用时传入的属性没有在`props`里面定义时，传入的属性将被绑定到`$attrs`属性内（`class`与`style`除外，他们会挂载到组件最外层元素上）。并可通过`v-bind="$attrs"`传入到内部组件中

> `$listeners`: 当组件被调用时，外部监听的这个组件的所有事件都可以通过`$listeners`获取到。并可通过`v-on="$listeners"`传入到内部组件中。

**注意：inheritAttrs:false;**

​        在Vue2.4.0,可以在组件定义中添加inheritAttrs：false，组件将不会把未被注册的props呈现为普通的HTML属性。但是在组件里我们可以通过其$attrs可以获取到没有使用的注册属性，如果需要，我们在这也可以往下继续传递。

孙子组件里获取到this.$attrs的值是除了子组件声明的元素！

实例：

```vue
<template>
  <div class="home">
    <mytest  :title="title" :massgae="massgae"></mytest>
  </div>
</template>
<script>
export default {
  name: 'home',
  data () {
    return {
      title:'title1111',
      massgae:'message111'
    }
  },
  components:{
    'mytest':{
      template:`<div>这是个h1标题{{title}}</div>`,
      props:['title'],
      data(){
        return{
          mag:'111'
        }
      },
      created:function(){
        console.log(this.$attrs)//注意这里
      }
    }
  }
}
</script>
```

![img](https://upload-images.jianshu.io/upload_images/7579449-a8a0b3970380eeeb.png?imageMogr2/auto-orient/strip|imageView2/2/w/909/format/webp)

### $listeners

$listeners可以让你在孙子组件改变父组件的值

```vue
<template>
    <div>
        <childcom :name="name" :age="age" :sex="sex" @testChangeName="changeName"></childcom>
    </div>
</template>
<script>
export default {
    'name':'test',
    props:[],
    data(){
        return {
            'name':'张三',
            'age':'30',
            'sex':'男'
        }
    },
    components:{
        'childcom':{
            props:['name'],
            template:`<div>
                <div>我是子组件   {{name}}</div>
                <grandcom v-bind="$attrs" v-on="$listeners"></grandcom>
            </div>`,
           
            components: {
                'grandcom':{
                    template:`<div>我是孙子组件-------<button @click="grandChangeName">改变名字</button></div>`,
                    methods:{
                        grandChangeName(){
                           this.$emit('testChangeName','kkkkkk')
                        }
                    }
                }
            }
        }
    },
    methods:{
        changeName(val){
            this.name = val
        }
    }
}
</script>
```

![img](media/webp.webp)

## 使用`require.context`实现前端工程自动化

`require.context`是一个`webpack`提供的Api,通过执行`require.context`函数获取一个特定的上下文,主要是用于实现自动化导入模块。

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a900bd9c4737a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

##### 通过`require.context`安装`Vue`组件

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a907e5f763528?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 自定义`v-model`

![img](https://user-gold-cdn.xitu.io/2020/6/11/172a1d912a292ee2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 使用`.sync`,更优雅的实现数据双向绑定

#### `.sync`与`v-model`的异同

相同点：

- 两者的本质都是语法糖，目的都是实现组件与外部数据的双向绑定
- 两个都是通过属性+事件来实现的

不同点(个人观点，如有不对，麻烦下方评论指出，谢谢)：

- 一个组件只能定义一个`v-model`,但可以定义多个`.sync`
- `v-model`与`.sync`对于的事件名称不同，`v-model`默认事件为`input`,可以通过配置`model`来修改，`.sync`事件名称固定为`update:属性名`

![img](https://user-gold-cdn.xitu.io/2020/6/12/172a70e9a027bdc8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 动态组件，让页面渲染更灵活

```vue
<template>
  <div class="info">
    <component :is="roleComponent" v-if="roleComponent" />
  </div>
</template>
```

## hookEvent,原来可以这样监听组件生命周期

#### 1. 内部监听生命周期函数

```js
  mounted() {
    this.chart = echarts.init(this.$el)
    // 请求数据，赋值数据 等等一系列操作...

    // 监听窗口发生变化，resize组件
    window.addEventListener('resize', this.$_handleResizeChart)
    // 通过hook监听组件销毁钩子函数，并取消监听事件
    this.$once('hook:beforeDestroy', () => {
      window.removeEventListener('resize', this.$_handleResizeChart)
    })
  },
```

#### 2. 外部监听生命周期函数

```vue
<template>
  <!--通过@hook:updated监听组件的updated生命钩子函数-->
  <!--组件的所有生命周期钩子都可以通过@hook:钩子函数名 来监听触发-->
  <custom-select @hook:updated="$_handleSelectUpdated" />
</template>
```

## 开发全局组件，你可能需要了解一下`Vue.extend`

`Vue.extend`是一个全局Api,平时我们在开发业务的时候很少会用到它，但有时候我们希望可以开发一些全局组件比如`Loading`,`Notify`,`Message`等组件时，这时候就可以使用`Vue.extend`。

#### 1. 开发`loading`组件

```vue
<template>
  <transition name="custom-loading-fade">
    <!--loading蒙版-->
    <div v-show="visible" class="custom-loading-mask">
      <!--loading中间的图标-->
      <div class="custom-loading-spinner">
        <i class="custom-spinner-icon"></i>
        <!--loading上面显示的文字-->
        <p class="custom-loading-text">{{ text }}</p>
      </div>
    </div>
  </transition>
</template>
<script>
export default {
  props: {
  // 是否显示loading
    visible: {
      type: Boolean,
      default: false
    },
    // loading上面的显示文字
    text: {
      type: String,
      default: ''
    }
  }
}
</script>
```

### 2.通过`Vue.extend`将组件转换为全局组件

#### 1. 改造`loading`组件，将组件的`props`改为`data`

```js
export default {
  data() {
    return {
      text: '',
      visible: false
    }
  }
}
```

#### 2. 通过`Vue.extend`改造组件

```js
// loading/index.js
import Vue from 'vue'
import LoadingComponent from './loading.vue'

// 通过Vue.extend将组件包装成一个子类
const LoadingConstructor = Vue.extend(LoadingComponent)

let loading = undefined

LoadingConstructor.prototype.close = function() {
  // 如果loading 有引用，则去掉引用
  if (loading) {
    loading = undefined
  }
  // 先将组件隐藏
  this.visible = false
  // 延迟300毫秒，等待loading关闭动画执行完之后销毁组件
  setTimeout(() => {
    // 移除挂载的dom元素
    if (this.$el && this.$el.parentNode) {
      this.$el.parentNode.removeChild(this.$el)
    }
    // 调用组件的$destroy方法进行组件销毁
    this.$destroy()
  }, 300)
}

const Loading = (options = {}) => {
  // 如果组件已渲染，则返回即可
  if (loading) {
    return loading
  }
  // 要挂载的元素
  const parent = document.body
  // 组件属性
  const opts = {
    text: '',
    ...options
  }
  // 通过构造函数初始化组件 相当于 new Vue()
  const instance = new LoadingConstructor({
    el: document.createElement('div'),
    data: opts
  })
  // 将loading元素挂在到parent上面
  parent.appendChild(instance.$el)
  // 显示loading
  Vue.nextTick(() => {
    instance.visible = true
  })
  // 将组件实例赋值给loading
  loading = instance
  return instance
}

export default Loading
```

#### 3. 在页面使用loading

```js
import Loading from './loading/index.js'
export default {
  created() {
    const loading = Loading({ text: '正在加载。。。' })
    // 三秒钟后关闭
    setTimeout(() => {
      loading.close()
    }, 3000)
  }
}
```

#### 4. 将组件挂载到`Vue.prototype`上面

```js
Vue.prototype.$loading = Loading
// 在export之前将Loading方法进行绑定
export default Loading

// 在组件内使用
this.$loading()
```

## watch

#### 随时监听，随时取消，了解一下`$watch`

有这样一个需求，有一个表单，在编辑的时候需要监听表单的变化，如果发生变化则保存按钮启用，否则保存按钮禁用。这时候对于新增表单来说，可以直接通过`watch`去监听表单数据(假设是`formData`),如上例所述，但对于编辑表单来说，表单需要回填数据，这时候会修改`formData`的值，会触发`watch`,无法准确的判断是否启用保存按钮。现在你就需要了解一下`$watch`

```js
export default {
  data() {
    return {
      formData: {
        name: '',
        age: 0
      }
    }
  },
  created() {
    this.$_loadData()
  },
  methods: {
    // 模拟异步请求数据
    $_loadData() {
      setTimeout(() => {
        // 先赋值
        this.formData = {
          name: '子君',
          age: 18
        }
        // 等表单数据回填之后，监听数据是否发生变化
        const unwatch = this.$watch(
          'formData',
          () => {
            console.log('数据发生了变化')
          },
          {
            deep: true
          }
        )
        // 模拟数据发生了变化
        setTimeout(() => {
          this.formData.name = '张三'
        }, 1000)
      }, 1000)
    }
  }
}
```

取消监听,需要取消的时候，执行 `unwatch()`即可取消

## 函数式组件，函数是组件？

纯展示性的业务组件，比如一些详情页面，列表界面等，它们有一个共同的特点是只需要将外部传入的数据进行展现，不需要有内部状态，不需要在生命周期钩子函数里面做处理，这时候你就可以考虑使用函数式组件。

#### 为什么使用函数式组件

1. 最主要最关键的原因是函数式组件不需要实例化，无状态，没有生命周期，所以渲染性能要好于普通组件
2. 函数式组件结构比较简单，代码结构更清晰

#### 函数式组件与普通组件的区别

1. 函数式组件需要在声明组件是指定functional
2. 函数式组件不需要实例化，所以没有`this`,`this`通过`render`函数的第二个参数来代替
3. 函数式组件没有生命周期钩子函数，不能使用计算属性，watch等等
4. 函数式组件不能通过$emit对外暴露事件，调用事件只能通过`context.listeners.click`的方式调用外部传入的事件
5. 因为函数式组件是没有实例化的，所以在外部通过`ref`去引用组件时，实际引用的是`HTMLElement`
6. 函数式组件的`props`可以不用显示声明，所以没有在`props`里面声明的属性都会被自动隐式解析为`prop`,而普通组件所有未声明的属性都被解析到`$attrs`里面，并自动挂载到组件根元素上面(可以通过`inheritAttrs`属性禁止)

##### 第一种函数组件

```vue
<!--在template 上面添加 functional属性-->
<template functional>
  <img :src="props.avatar ? props.avatar : 'default-avatar.png'" />
</template>
<!--根据上一节第六条，可以省略声明props-->
```

##### 第二种函数组件

```js
export default {
  // 通过配置functional属性指定组件为函数式组件
  functional: true,
  // 组件接收的外部属性
  props: {
    avatar: {
      type: String
    }
  },
  /**
   * 渲染函数
   * @param {*} h
   * @param {*} context 函数式组件没有this, props, slots等都在context上面挂着
   */
  render(h, context) {
    const { props } = context
    if (props.avatar) {
      return <img src={props.avatar}></img>
    }
    return <img src="default-avatar.png"></img>
  }
}
```

