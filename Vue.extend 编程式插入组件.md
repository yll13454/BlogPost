步骤：

1. 通过 Vue.extend() 创建构造器
2. 通过 Vue.$mount() 挂载到目标元素上

### Vue.extend()

首先来定义这个弹框的结构和样式，就是正常的写组件即可

```vue
<template>
  <div class="grid">
      <h1 class="head">这里是标题</h1>
      <div @click="close">{{ cancelText }}</div>
      <div @click="onSureClick">{{ sureText }}</div>
  </div>
</template>
<script>
export default {
  props:{
    close:{
      type:Function,
      default:()=>{}
    },
    cancelText:{
      type:String,
      default:'取消'
    },
    sureText:{
      type:String,
      default:'确定'
    }
  },
  methods:{
    onSureClick(){
      // 其他逻辑
      this.close()
    }
  }
};
</script>
```

```js
export function extendComponents(component,callback){
  const Action = Vue.extend(component)
  const div = document.createElement('div')
  document.body.appendChild(div)
  const ele = new Action({
    propsData:{
      cancelText:'cancel',
      sureText:'sure',
      close:()=>{
        ele.$el.remove()
        callback&&callback()
      }
    }
  }).$mount(div)
}
```

**注意：**

1. Vue.extend 获得是一个构造函数，可以通过实例化生成一个 Vue 实例
2. 实例化时可以向这个实例传入参数，但是`需要注意`的是 props 的值需要通过 propsData 属性来传递
3. 得到 Vue 实例后，我们需要通过一个目标元素来挂载它，有人首先会想到挂载到 `#app` 上，这个挂载的过程是将`目标元素的内容全部替换`，所以一旦挂载到 `#app` 上，该元素的所有子元素都会消失被替换
4. 针对第3点，所以创建了一个 div 元素插入到 body 中，我们将想要挂载的内容替换到这个div上

## Vue.extend 和 Vue.component component 的区别

1. Vue.component component两者都是需要先进行组件注册后，然后在 template 中使用注册的标签名来实现组件的使用。Vue.extend 则是编程式的写法
2. 关于组件的显示与否，需要在父组件中传入一个状态来控制 或者 在组件外部用 v-if/v-show 来实现控制，而 Vue.extend 的显示与否是`手动`的去做组件的`挂载和销毁`。
3. Vue.component component 在组件中需要使用 slot 等自定义UI时更加灵活，而 Vue.extend 由于没有 template的使用，没有slot 都是通过 props 来控制UI，更加局限一些。

## [Vue.extend用法](https://www.cnblogs.com/xuzhudong/p/8631088.html)

### 1. 第一种用法--挂在到元素上

![img](https://images2018.cnblogs.com/blog/1112801/201803/1112801-20180323162048998-2072587600.png)

### 2. 第二种用法--将组件注册到`Vue.component` 全局方法里面

![img](https://images2018.cnblogs.com/blog/1112801/201803/1112801-20180323162604149-993003002.png)

![img](https://images2018.cnblogs.com/blog/1112801/201803/1112801-20180323163205903-1979539273.png)

### 3. 第三种方法--将组件注册为局部组件

![img](https://images2018.cnblogs.com/blog/1112801/201803/1112801-20180323163007267-657315594.png)

![img](https://images2018.cnblogs.com/blog/1112801/201803/1112801-20180323163224922-18907175.png)