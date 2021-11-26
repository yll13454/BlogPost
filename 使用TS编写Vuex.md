## 使用TS编写Vuex

vuex整体对ts的支持比较蹩脚，这是以前架构问题引起的，我们一起来感受一下：

### 创建Store

创建store实例，`store/index.ts`

```ts
import { createStore, Store } from "vuex";
import { State } from "./vuex";

const store = createStore({
  state: {
    counter: 0,
  },
});

export default store;
复制代码
```

引入vue，`main.ts`

```ts
createApp(App).use(store).mount("#app");
复制代码
```

使用，Comp.vue

```ts
import { mapState } from "vuex";

export default defineComponent({
  computed: {
    // 映射state counter
    ...mapState(['counter']),
    doubleCounter(): number {
      // $store已经有类型了
      return this.$store.state.counter * 2;
    },
  },
}
复制代码
```

### $store类型化

我们希望`this.$store`是有明确类型的, 这需要为组件选项添加一个明确类型的`$store`属性，可以为`ComponentCustomProperties`扩展`$store`属性，`store/vuex.d.ts`

```ts
import { ComponentCustomProperties } from "vue";
import { Store } from "vuex";

// declare your own store states
export interface State {
  counter: number;
}

declare module "@vue/runtime-core" {
  // provide typings for `this.$store`
  interface ComponentCustomProperties {
    $store: Store<State>;
  }
}
复制代码
```

### useStore()类型化

`setup`中使用`useStore`时要类型化，共需要三步：

1. 定义 `InjectionKey`
2. app安装时提供`InjectionKey`
3. 传递 `InjectionKey` 给 `useStore`

定义一个`InjectionKey`，约束`Store`中`State`类型，store/index.ts

```ts
import { InjectionKey } from "vue";
import { State } from "./vuex";

// define injection key
export const key: InjectionKey<Store<State>> = Symbol();
复制代码
```

main.ts中作为参数2传入vuex插件

```ts
import { key } from "./store";
// 作为参数2传入key
createApp(App).use(store, key).mount("#app");
复制代码
```

使用时，store就可以有明确类型了，CompSetup.vue

```ts
import { useStore } from 'vuex'
import { key } from '../store'

const store = useStore()
const counter = computed(() => store.state.counter);
复制代码
```

### 简化使用

封装useStore，避免每次导入key，store/index.ts

```ts
import { useStore as baseUseStore } from "vuex";
export function useStore() {
  return baseUseStore(key);
}
复制代码
```

使用变化，CompSetup.vue

```ts
import { useStore } from '../store'
const store = useStore()
复制代码
```

### 模块化

创建模块文件，store/modules/todo.ts

```ts
import { Module } from "vuex";
import { State } from "../vuex";
import type { Todo } from "../../types";

const initialState = {
  items: [] as Todo[],
};

export type TodoState = typeof initialState;

export default {
  namespaced: true,
  state: initialState,
  mutations: {
    initTodo(state, payload: Todo[]) {
      state.items = payload;
    },
    addTodo(state, payload: Todo) {
      state.items.push(payload)
    }
  },
  actions: {
    initTodo({ commit }) {
      setTimeout(() => {
        commit("initTodo", [
          {
            id: 1,
            name: "vue3",
            completed: false,
          },
        ]);
      }, 1000);
    }
  },
} as Module<TodoState, State>;
复制代码
```

引入子模块，store/index.ts

```ts
import todo from "./modules/todo";
const store = createStore({
  modules: {
    todo,
  },
});
复制代码
```

状态中添加模块信息，vuex.d.ts

```ts
import type { TodoState } from "./modules/todo";

export interface State {
  todo?: TodoState;
}
复制代码
```

组件中使用，Comp.vue

```ts
export default {
  data() {
    return {
      // items: [] as Todo[],
    };
  },
  computed: {
    items(): Todo[] {
      return this.$store.state.todo!.items
    }
  },
  methods: {
    addTodo(todo: Todo) {
      // this.items.push(todo);
      this.$store.commit("todo/addTodo", todo);
      this.todoName = "";
    },
  },
}
复制代码
```

setup中使用，CompSetup.vue

```ts
const items = computed(() => store.state.todo!.items)
store.dispatch('todo/initTodo')

function addTodo(todo: Todo) {
  // items.value.push(todo);
  store.commit('todo/addTodo', todo)
  todoName.value = "";
}
复制代码
```

## 总结

`vue3+ts`体验中规中矩，跟`react`相比还有差距，尤其`vuex`这块支持比较弱，仅能做到`state`类型支持，`getters`依然`any`，子模块`mutations`和`actions`更是完全抓瞎，这个是以前架构问题，估计以后会有`vuex5`来解决。

ts显然还是做开源库和框架更好一点，业务编写不是特别必要。


作者：杨村长
链接：https://juejin.cn/post/6979034498352545829
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。