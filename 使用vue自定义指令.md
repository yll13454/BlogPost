[Vue](http://mp.weixin.qq.com/s?__biz=MzAxODE4MTEzMA==&mid=2650084449&idx=1&sn=534f81cab5b7882e856d5c0221cf8141&chksm=83db8904b4ac001264fcca969aeac10728c032cf888b70c5bb3e63298082008b51d70374cb17&scene=21#wechat_redirect) 自定义指令有全局注册和局部注册两种方式。先来看看注册全局指令的方式，通过 `Vue.directive( id, [definition] )` 方式注册全局指令。然后在入口文件中进行 `Vue.use()` 调用。

批量注册指令，新建 `directives/index.js` 文件

```js
import copy from './copy'
import longpress from './longpress'
// 自定义指令
const directives = {
  copy,
  longpress,
}
 
export default {
  install(Vue) {
    Object.keys(directives).forEach((key) => {
      Vue.directive(key, directives[key])
    })
  },
}
```

在 `main.js` 引入并调用

```js
import Vue from 'vue'
import Directives from './JS/directives'
Vue.use(Directives)
```

钩子函数（可选）：

- bind: 只调用一次，指令第一次绑定到元素时调用，可以定义一个在绑定时执行一次的初始化动作。
- inserted: 被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于 document 中）。
- update: 被绑定元素所在的模板更新时调用，而不论绑定值是否变化。通过比较更新前后的绑定值。
- componentUpdated: 被绑定元素所在模板完成一次更新周期时调用。
- unbind: 只调用一次， 指令与元素解绑时调用。

### 实用的 [Vue](http://mp.weixin.qq.com/s?__biz=MzAxODE4MTEzMA==&mid=2650084124&idx=1&sn=34c10318a887fba274e389ad29dce1af&chksm=83db8e79b4ac076fcf41bf26d65fbab59b8c02b175ff533169ca064e80cbc1be3527a29c4cd8&scene=21#wechat_redirect) 自定义指令

- 复制粘贴指令 `v-copy`
- 长按指令 `v-longpress`
- 输入框防抖指令 `v-debounce`
- 禁止表情及特殊字符 `v-emoji`
- 图片懒加载 `v-LazyLoad`
- 权限校验指令 `v-premission`
- 实现页面水印 `v-waterMarker`
- 拖拽指令 `v-draggable=

[8个非常实用的Vue自定义指令](https://www.cnblogs.com/lzq035/p/14183553.html)

