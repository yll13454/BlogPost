# 

# [Vue解决V-HTML指令潜在的XSS攻击（'v-html' directive can lead to XSS attack.）](https://www.jianshu.com/p/8167d44122a0)

XSS是跨站脚本攻击（Cross-Site Scripting）的简称。

XSS是一种注入脚本式攻击，攻击者利用如提交表单、发布评论等方式将事先准备好的恶意脚本注入到那些良性可信的网站中。

当其他用户进入该网站后，脚本就在用户不知情的情况下偷偷地执行了，这样的脚本可能会窃取用户的信息、修改页面内容、或者伪造用户执行其他操作等等，后果不可估量。

发送到Web浏览器的恶意内容通常采用JavaScript代码片段的形式，但也可能包括HTML，Flash或浏览器可能执行的任何其他类型的代码。

## 法1：

### 一、下载依赖

```undefined
cnpm install vue-dompurify-html
```

### 二、main.js中引入xss包并挂载到vue原型上

```jsx
import VueDOMPurifyHTML from 'vue-dompurify-html'
Vue.use(VueDOMPurifyHTML)
```

### 三、使用

```xml
 <div v-dompurify-html="src" />
```

## 法2：

### 一、下载依赖

```undefined
npm install xss --save
```

### 二、main.js中引入xss包并挂载到vue原型上

```jsx
import xss from 'xss'
Vue.prototype.xss = xss
```

### 三、在vue.config.js中覆写html指令

```jsx
  chainWebpack: config => {
    config.module
      .rule('vue')
      .use('vue-loader')
      .loader('vue-loader')
      .tap(options => {
        options.compilerOptions.directives = {
          html(node, directiveMeta) {
            (node.props || (node.props = [])).push({
              name: 'innerHTML',
              value: `xss(_s(${directiveMeta.value}))`
            })
          }
        }
        return options
      })
  }
```

### 四、使用

```xml
 <div v-dompurify-html="src" />
```

# [解决v-html指令潜在的xss攻击](https://blog.csdn.net/lingxiaoxi_ling/article/details/105851736)

### html指令的运行原理

v-html指令源码

```javascript
// /vue/src/platforms/web/compiler/directives/html.js
export default function html (el: ASTElement, dir: ASTDirective) {
  if (dir.value) {
    addProp(el, 'innerHTML', `_s(${dir.value})`)
  }
}
```

