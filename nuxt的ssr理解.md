[nuxt的asyncData为什么会报跨域错误，奇葩啊~~~](https://segmentfault.com/q/1010000022612958)

nuxt的asyncData调用会出现以下跨域报错，但是刷新后就好了。而且每次路由切换后只要有asyncData都会有跨域报错，但是刷新后都会好。大佬告诉我下是为啥吗？

## 原因

你对nuxt的ssr理解有误。asyncData并不是每次都从服务端渲染，只有当页面刷新时才从服务端渲染，而在客户端点击链接跳转时并不从服务端渲染，而是和标准vue的spa应用一样，从客户端进行渲染。

nuxt[官方英文文档](https://link.segmentfault.com/?url=https%3A%2F%2Fnuxtjs.org%2Fapi%2F)有说明，中文文档没有：

> `asyncData` is called every time before loading the page component and is only available for such. It will be called server-side once (on the first request to the Nuxt app) and client-side when navigating to further routes.

