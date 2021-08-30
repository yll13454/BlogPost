# NProgress*.js*

```
// 下载安装
npm i nprogress -S

// 基本用法
NProgress.start();			// 开启进度条
NProgress.done();			// 关闭进度条
```

## 在main.js中引入

```
import NProgress from 'nprogress'; // 进度条
import 'nprogress/nprogress.css'; //这个样式必须引入


NProgress.inc(0.2);//递增进度条
NProgress.configure({ easing: 'ease', speed: 500, showSpinner: false });

1.如果想路由跳转有progress，加在vue-router的beforeEach和afterEach中
router.beforeEach((to,from,next) => {
  NProgress.start() 
  next()
})

router.afterEach(() => {
  NProgress.done()
})
```

## 修改进度条颜色

在App.vue中的style中增加：

```js
<style>
#nprogress .bar {
  background: blue !important;
}
</style>
```

### 常用配置

**递增：**要递增进度条，只需使用`.inc()`。这使它以随机量递增。这将永远不会达到100％：将其用于每次图像加载（或类似加载）。

```
NProgress.inc();
```

```
NProgress.inc(0.2);     //这将获取当前状态值并添加0.2直到状态为0.994
```

> 即每次增加一点点，但永远不会到百分之百

**配置项：**

#### `easing` 和 `speed`

使用*缓动*（CSS缓动字符串）和*速度*（以毫秒为单位）调整动画设置。（默认：`ease`和`200`）

```
NProgress.configure({ easing: 'ease', speed: 500 });
```

#### `showSpinner`

通过将加载微调器设置为false来关闭它。（默认值：`true`）

```
NProgress.configure({ showSpinner: false });
```




#### 在页面切换中使用

在~/router/index.js 【路由配置】文件中:

```
import Vue from 'vue'
import VueRouter from 'vue-router'
//引入nprogress 进度条插件
import NProgress from 'nprogress'

Vue.use(VueRouter);

// 简单配置  进度条,可以不配置：在axios中我们不再做配置，以用来区分。
NProgress.inc(0.2)
NProgress.configure({ easing: 'ease', speed: 500, showSpinner: false })


export const constRouter = [
    {
        path: '/login',
        component: () => import('@/views/login/Login'),
    },
    ...
]

const router = new VueRouter({
    mode: 'history',
    base: process.env.BASE_URL,
    routes: constRouter
})

// 页面路由刚开始切换的时候
router.beforeEach(async (to,from,next) => {
	// 开启进度条
	NProgress.start();
})

// 页面路由切换完毕的时候
router.afterEach(() => {
	// 关闭进度条
    NProgress.done()
})


export default router
复制代码
```

#### 在接口请求中使用

在~/api/index.js 【axios请求配置】文件中:

```
import axios from 'axios'
//引入nprogress 进度条插件
import NProgress from 'nprogress'

// 创建axios实例
const service = axios.create({
    baseURL: process.env.VUE_APP_BASE_API, //URL地址   环境变量文件
    timeout: 5000 ,//超时
})

// 请求拦截器
service.interceptors.request.use(
    config => {
    	// 开启进度条
		NProgress.start();
        return config
    },
    error => {
        return Promise.reject(error)
    }
)

// 响应拦截器
service.interceptors.response.use(
    response =>{
        // 关闭进度条
        NProgress.done()
    	return Promise.reject(response)
    },
    error => {
        // 关闭进度条
        NProgress.done()
        return Promise.reject(error)
    }
)

export default service;
```


作者：ZhangHaoran
链接：https://juejin.cn/post/6917801127065550856
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。