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

### defineProps 、 defineEmit 和 useContext