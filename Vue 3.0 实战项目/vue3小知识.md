h函数

```js
import { h, resolveComponent } from 'vue'

h(ElDivider, { direction: 'vertical' }),
```

返回一个”虚拟节点“，通常缩写为 **VNode**：一个普通对象，其中包含向 Vue 描述它应在页面上渲染哪种节点的信息，包括所有子节点的描述。它的目的是用于手动编写的[渲染函数](https://vue3js.cn/docs/zh/guide/render-function.html)：

```
render() {
  return Vue.h('h1', {}, 'Some title')
}
```

接收三个参数：`type`，`props` 和 `children`

```js
h('div', {}, [
  'Some text comes first.',
  h('h1', 'A headline'),
  h(MyComponent, {
    someProp: 'foobar'
  })
])
```

### 奇怪写法

```
<script setup>
      import { ref } from 'vue'
      ref: foo = 1
      </script>
```

```
defineProps<{
  name: string;
  phoneNumber: number;
  userInfo: object;
  tags: string[];
}>();
```

```
interface UserInfo {
  id: number;
  age: number;
}

defineProps<{
  name: string;
  userInfo: UserInfo;
}>();



// name 是可选
defineProps<{
  name?: string;
  tags: string[];
}>();
```

泛型里不能设置默认值

## 什么是 script-setup

```html
<!-- 使用 script-setup 格式 -->
<script setup lang="ts">
  // ...
</script>
```

在 script-setup 模式下，只需要导入组件即可，编译器会自动识别并启用。

```html
<!-- 使用 script-setup 格式 -->
<script setup lang="ts">
import Header from '@cp/Header.vue'
</script>
```

## 什么是 props 和 emits

```html
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: [ 'name' ],
  emits: [ 'changeName' ],
  setup (props, { emit }) {

    setTimeout(() => {
      emit('changeName', 'Tom');
    }, 1000);
    
  }
})
</script>
```

## defineProps 和 defineEmit

使用限制，只能用于 script-setup 。

## defineProps

defineProps 是一个方法，内部返回一个对象，也就是挂载到这个组件上的所有 props ，它和普通的 props 用法一样，如果不指定为 prop， 则传下来的属性会被放到 attrs 那边去。

### [#](https://chengpeiquan.com/article/vue3-script-setup.html#基础用法)基础用法

```ts
const props = defineProps([
  'name',
  'userInfo',
  'tags'
])

console.log(props.name);
```

### 通过构造函数进行检查

```ts
defineProps({
  name: {
    type: String,
    required: false,
    default: 'Petter'
  },
  userInfo: Object,
  tags: Array
});
```

### 使用类型注解进行检查

使用 TypeScript 的类型注解。

```tsx
defineProps<{
  name: string;
  phoneNumber: number;
  userInfo: object;
  tags: string[];
}>();
```

在这里，不再使用构造函数校验，而是需要遵循使用 TypeScript 的类型，比如字符串是 string，而不是 String。

其中，举例里的 userInfo 是一个对象，你可以简单的指定为 object，也可以先定义好它对应的类型，再进行指定：

```ts
interface UserInfo {
  id: number;
  age: number;
}

defineProps<{
  name: string;
  userInfo: UserInfo;
}>();
```

如果你想对某个数据设置为可选，也是遵循 TS 规范，通过英文问号 `?` 来允许可选：

```ts
// name 是可选
defineProps<{
  name?: string;
  tags: string[];
}>();
```

> *需要强调的一点是：这两种校验方式只能二选一，否则会引起程序报错*

## defineEmit

需要通过字面量来定义 emits，最基础的用法也是传递一个 string[] 数组进来，把每个 emit 的名称作为数组的 item 。

```ts
// 获取 emit
const emit = defineEmit(['chang-name']);

// 调用 emit
emit('chang-name', 'Tom');
```

## useContext

在标准组件写法里，setup 函数默认支持两个入参：

| 参数    | 类型   | 含义                   |
| :------ | :----- | :--------------------- |
| props   | object | 由父组件传递下来的数据 |
| context | object | 组件的执行上下文       |

这里的第二个参数 context，在 script-setup 写法里，就需要通过 useContext 来获取，一样的，记得先导入依赖：

```ts
// 导入 useContext 组件
import { useContext } from 'vue'

// 获取 context
const ctx = useContext();

// 打印 attrs
console.log(ctx.attrs);
```

你也可以对它进行解构，直接获取到内部的数据：

```ts
// 直接获取 attrs
const { attrs } = useContext();
```

























# 引入组件报错

![image-20210429122234279](media/image-20210429122234279.png) 

最重要的就是将以前的

```
import echarts from 'echarts'
```

改为

```
import * as echarts from 'echarts'
```

