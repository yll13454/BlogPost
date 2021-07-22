## Vue插槽slot的基本使用

### 单个插槽 | 匿名插槽

```vue
<template>
  <div class= 'child'>
      <slot></slot>
  </div>
</template>
```

### 编译作用域 （父组件在子组件`<slot></slot>`处插入 data）

```js
// 子组件 ： (假设名为：child)
<template>
  <div class= 'child'>
       <slot></slot>
  </div>
</template>

new Vue({
  el:'child',
  data:{
    child:'子组件'
  }
})

// 父组件：（引用子组件 child）

<template>
  <div class= 'app'>
     <child> {{ child }}</child>
  </div>
</template>
复制代码
```

> 直接传入子组件内的数据是不可以的。因为：父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

### 后备内容 (子组件`<slot></slot>`设置默认值)

> 所谓的后背内容，其实就是slot的默认值，有时我没有在父组件内添加内容，那么 slot就会显示默认值，如：

```js
//子组件 ： (假设名为：child)
<template>
  <div class='child'>
      <slot>这就是默认值</slot>
  </div>
</template>
```

### 具名插槽 （子组件多个`<slot></slot>`对应插入内容）

> 有时候，也许子组件内的slot不止一个，那么我们如何在父组件中，精确的在想要的位置，插入对应的内容呢？ 给插槽命一个名即可，即添加name属性。

```js
//子组件 ： (假设名为：child)
<template>
  <div class= 'child'>
      <slot name='one'> 这就是默认值1</slot>
      <slot name='two'> 这就是默认值2 </slot>
      <slot name='three'> 这就是默认值3 </slot>
  </div>
</template>
复制代码
```

> 父组件通过，或`slot="name"（旧语法）`，`v-slot:name`或`#name（新语法）` 的方式添加内容：

```js
//父组件：（引用子组件 child）
<template>
  <div class= 'app'>
     <child> 
        <template v-slot:"one"> 这是插入到one插槽的内容 </template>
        <template v-slot:"two"> 这是插入到two插槽的内容 </template>
        <template v-slot:"three"> 这是插入到three插槽的内容 </template>
     </child>
  </div>
</template>
```

### 作用域插槽 (父组件在子组件`<slot></slot>`处使用子组件 data)

**slot-scope**

```
//子组件 ： (假设名为：child)
<template>
  <div class= 'child'>
      <slot name= 'one' :value1='child1'> 这就是默认值1</slot>    //绑定child1的数据
      <slot :value2='child2'> 这就是默认值2 </slot>  //绑定child2的数据，这里我没有命名slot
  </div>           
</template>

new Vue({
  el:'child',
  data:{
    child1:'数据1',
    child2:'数据2'
  }
})

//父组件：（引用子组件 child）
<template>
  <div class='app'>
     <child> 
        <template v-slot:one='slotone'>  
           {{ slotone.value1 }}    // 通过v-slot的语法 将子组件的value1值赋值给slotone 
        </template>
        <template v-slot:default='slotde'> 
           {{ slotde.value2 }}  // 同上，由于子组件没有给slot命名，默认值就为default
        </template>
     </child>
  </div>
</template>
```

## Slot插槽原理

```js
//子组件 ： (假设名为：child)
<template>
  <div class= 'child'>
      <slot :value1='child1' :value2='child1'></slot>
      <slot name='one' :value1='child2' :value2='child2'></slot>
  </div>           
</template>

new Vue({
  el:'child',
  data:{
    child1: '子数据1',
    child2: '子数据2'
  }
})

//父组件：（引用子组件 child）
<template>
  <div class='app'>
     <child> 
         <template v-slot:default='slotde'> 
            插入默认 slot 中{{ slotde.value1 }}{{ slotde.value2 }}
        </template>
        <template v-slot:one='slotone'> 
            插入one slot 中{{ slotone.value1 }}{{ slotone.value2 }}
        </template>
     </child>
  </div>
</template>
复制代码
```

1. 过程很复杂，这里就通俗点讲了，父组件先解析，遇到作用域插槽，会将此插槽封装成一个函数保存到子元素 `child` 下

```js
{    
 tag: "div",    
  children: [{        
     tag: "child"
     scopeSlots:{            
         default (data) { // 记住这个data参数               
             return ['插入one slot 中插入默认 slot 中' + data.value1 + data.value2]
         },
         one (data) { // 记住这个data参数             
             return ['插入one slot 中' + data.value1 + data.value2]
         }
     }
    }]
}
复制代码
```

2.轮到子组件解析了，这个时候_t函数又登场了，并且子组件将对应的插槽数据包装成一个对象，传进_t函数

```js
{    
 tag: "div",    
   children: [
     '我在子组件里面',
      _t('default',{value1: '子数据1', value2: '子数据1'}),
      _t('one',{value1: '子数据2', value2: '子数据2'})
      
    ]
  }
复制代码
```

> 接下来就是_t内部执行，包装后的对象被当做data参数传入了scopeSlots中的对应的各个函数，解析成：

```js
{    
  tag: "div",    
   children: [
      '我在子组件里面', 
      '插入默认 slot 中 子数据1 子数据1',
      '插入one slot 中 子数据2 子数据2'
   ]
}
```


作者：Sunshine_Lin
链接：https://juejin.cn/post/6949848530781470733
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### $slots

解析后的`节点VNode`对象

 console.log(this.$slots) // 看看里面有啥

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/db269ceae0b24ed79cfe372c31d88bde~tplv-k3u1fbpfcp-watermark.image) 

`$slots`是一个`Map`，`key`是各个插槽的名称（匿名插槽的`key`为`default`），key对应的value都是各个插槽下面的VNode节点