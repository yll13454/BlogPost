# vueRouter4.0设置404页面

4.0以后的路由得用下面这种方式才能设置通配符*来匹配错误的页面

```js
{    
    path: '/:pathMatch(.*)*', 
    name: '404', 
    component: () => import('@/views/error/404.vue')
}
```

