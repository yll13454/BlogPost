![img](media/ff94cb96d5684b62aac54b1438785788~tplv-k3u1fbpfcp-zoom-1.image)

### Teleport

要使用 `<teleport>`，首先要在页面上添加一个元素，我们要将模态内容渲染到该元素下面。

```html
<body>
  <div id="app">
    <h3>Tooltips with Vue 3 Teleport</h3>
  </div>
  <div id="endofbody"></div>
</body>
```

\--

```vue
<template>
  <button @click="openModal">
    Click to open modal! (With teleport!)
  </button>
  <teleport to="#endofbody">
    <div v-if="isModalOpen" class="modal">
      ...
    </div>
  </teleport>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {
    const isModalOpen = ref(false)
    const openModal = function () {
      isModalOpen.value = true
    }
    return {
      isModalOpen,
      openModal
    }
  }
}
</script>
```

### Suspense

`<Suspense>`是一个特殊的组件，它将呈现回退内容，而不是对于的组件，直到满足条件为止，这种情况通常是组件`setup`功能中发生的异步操作或者是异步组件中使用。例如这里有一个场景，父组件展示的内容包含异步的子组件，异步的子组件需要一定的时间才可以加载并展示，这时就需要一个组件处理一些占位逻辑或者加载异常逻辑，要用到 `<Suspense>`，例如：

```vue
// vue3.x
<Suspense>
  <template >
    <Suspended-component />
  </template>
  <template #fallback>
    Loading...
  </template>
</Suspense>
```

假设 `<Suspended-component>`是一个异步组件，直到它完全加载并渲染前都会显示占位内容：Loading，这就是 `<Suspense>`的简单用法，该特性和Fragment以及 一样，灵感来自React

### vue3.x router

```tsx
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

#### scrollBehavior滚动行为

```tsx
// vue2.x router
const router = new Router({
  base: process.env.BASE_URL,
  mode: 'history',
  scrollBehavior: () => ({ x: 0, y: 0 }),
  routes
})

// vue3.x router
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes,
  scrollBehavior(to, from, savedPosition) {
    // scroll to id `#app` + 200px, and scroll smoothly (when supported by the browser)
    return {
      el: '#app',
      top: 0,
      left: 0,
      behavior: 'smooth'
    }
  }
})
```

#### 路由组件跳转

vue2.x使用路由选项redirect设置路由自动调整，vue3.x中移除了这个选项，将在子路由中添加一个空路径路由来匹配跳转

```
// vue2.x router
[
  {
    path: '/',
    component: Layout,
    name: 'WebHome',
    meta: { title: '平台首页' },
    redirect: '/dashboard', // 这里写跳转
    children: [
      {
        path: 'dashboard',
        name: 'Dashboard',
        meta: { title: '工作台' },
        component: () => import('../views/dashboard/index.vue')
      }
    ]
  }
]

// vue3.x router
[
  {
    path: '/',
    component: Layout,
    name: 'WebHome',
    meta: { title: '平台首页' },
    children: [
      { path: '', redirect: 'dashboard' }, // 这里写跳转
      {
        path: 'dashboard',
        name: 'Dashboard',
        meta: { title: '工作台' },
        component: () => import('../views/dashboard/index.vue')
      }
    ]
  }
]
```

#### 捕获所有路由：/:catchAll(.*)

捕获所有路由 `( /* )` 时，现在必须使用带有自定义正则表达式的参数进行定义：`/:catchAll(.*)`

```
// vue2.x router
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '/user/:a*' },
  ]
})

// vue3.x router
const router = createRouter({
  history: createWebHistory(),
  routes: [
    { path: '/user/:a:catchAll(.*)', component: component },
  ]
})
复制代码
```

当路由为 `/user/a/b` 时，捕获到的 `params` 为 `{"a": "a", "catchAll": "/b"}`

#### router.resolve

`router.match` 与 `router.resolve` 合并在一起为 `router.resolve`，但签名略有不同

```tsx
// vue2.x router
...
resolve ( to: RawLocation, current?: Route, append?: boolean) {
  ...
  return {
    location,
    route,
    href,
    normalizedTo: location,
    resolved: route
  }
}

// vue3.x router
function resolve(
    rawLocation: Readonly<RouteLocationRaw>,
    currentLocation?: Readonly<RouteLocationNormalizedLoaded>
  ): RouteLocation & { href: string } {
  ...
  let matchedRoute = matcher.resolve(matcherLocation, currentLocation)
  ...
  return {
    fullPath,
    hash,
    query: normalizeQuery(rawLocation.query),
    ...matchedRoute,
    redirectedFrom: undefined,
    href: routerHistory.base + fullPath,
  }
}
```


作者：j2ime
链接：https://juejin.cn/post/6867123283427328008
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。