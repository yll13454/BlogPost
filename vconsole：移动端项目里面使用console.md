#### main.js中引入 

```
npm run vconsole --save
```

在main.js同级建立一个vconsole.js得文件

```javascript
import Vconsole from 'vconsole'
const vConsole = new Vconsole()
export default vConsole
```

在main.js中引入(`看这里`得两行得是新加的)

```javascript
  import Vue from 'vue'
  import App from './App'
  import router from './router'
  import vConsole from './vconsole'  // 看这里
  Vue.config.productionTip = false
  /* eslint-disable no-new */
  new Vue({
    el: '#app',
    router,
    vConsole, // 看这里
    components: { App },
    template: '<App/>'
  })
```


![img](media/51b2eca1b8bf4184a38cdb74cc59e85ftplv-k3u1fbpfcp-watermark.awebp)

#### 第二种 使用webpack的里面的包 （`依赖于webpack`)

```
npm install vconsole-webpack-plugin --save-dev
```

在vue.config.js里面写入

```javascript
const VConsolePlugin = require('vconsole-webpack-plugin')

const vcPlugin = new VConsolePlugin({
  filter: [],
  enable: process.env.NODE_ENV !== 'production', // 生产环境不需要vconsole
})
module.exports = {
  configureWebpack: config => {
    config.externals = {
    }
    config.plugins = [vcPlugin, ...config.plugins]
  },
}
```


作者：可爱子
链接：https://juejin.cn/post/6922416050545901582
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。