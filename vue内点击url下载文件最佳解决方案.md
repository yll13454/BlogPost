*开发中经常遇到这样的功能,用户将文件或附件上传到服务器,后端将文件放到ftp或是其他位置,在前端页面内有下载的入口,有时候,后端返回的是blob,这种情况当然是最好的,但是为了方便,后端也可能返回文件所在位置的url,这时,对前端来说,可能遇到一些问题,比如,下载文件时候浏览器可能会出现闪动,下载图片,json文件等浏览器支持的文件时候,不会下载,而是直接在浏览器内打开这类文件,下面的方法可以完美解决此类问题*

解决方案:

- 封装自定义指令
- 将url转成bold,在创建a标签下载blob

代码实现

1. 在src 下面的 directive 文件夹下新建目录 **down-load-url**
2. down-load-url / index.js文件

```javascript
/*
 * 后端返回文件的url,前端创建a标签来下载
 *
 *  1. 解决了若文件为图片或浏览器支持的格式类型,点击下载会直接打开文件的问题,
 *  2. 下载文件时,浏览器会有闪动的问题
 *
 *  页面内使用
 *  1. 引入指令 import downLoad from '@/directive/down-load-url'
 *  2. 注册指令 directives:{downLoad}
 *  3. 使用,在要下载按钮上以指令的形式使用 例如: <el-button v-downLoad="url">下载</el-button>
 */

import downLoad from './downLoad'

const install = function(Vue) {
  Vue.directive('downLoadUrl', downLoad)
}

downLoad.install = install

export default downLoad
```

1. down-load-url / downLoad.js文件

```javascript
export default {
  bind(el, binding) {
    el.addEventListener('click', () => {
      console.log(binding.value) // url
      const a = document.createElement('a')
      //   let url = baseUrl + binding.value // 若是不完整的url则需要拼接baseURL
      const url = binding.value // 完整的url则直接使用
      // 这里是将url转成blob地址，
      fetch(url).then(res => res.blob()).then(blob => { // 将链接地址字符内容转变成blob地址
        a.href = URL.createObjectURL(blob)
        console.log(a.href)
        a.download = '' // 下载文件的名字
        // a.download = url.split('/')[url.split('/').length -1] //  // 下载文件的名字
        document.body.appendChild(a)
        a.click()
      })
    })
  }
}
```

