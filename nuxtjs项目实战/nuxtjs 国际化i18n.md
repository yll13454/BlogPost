## 1. 设置国际化匹配文字：

locales/zh.json: (locales/en.json 英文同理)

```js
{
  "topbar": {
    "home": "首页",
    "pin": "沸点",
    "topic": "话题",
    "book": "小册",
    "search": "搜索"
  },
  "menu": {
    "home": "我的主页",
    "label": "标签管理",
    "logout": "登出",
    "login": "登录"
  }
}
123456789101112131415
```

## 2. 设置vue-i18n国际化插件：(用户通过改变locale，使$t(‘topbar.name’)输出不同语言)

plugins/i18n.js:

```javascript
import Vue from "vue"
import VueI18n from "vue-i18n"
Vue.use(VueI18n)
export default ({ app }) => {
  app.i18n = new VueI18n({
    locale: app.$cookies.get('lang') || 'zh',  //通过cookie设置国际化（用户选择一次终身不切换）
    fallbackLocale: 'zh',   //默认国际化
    messages: {
      en: require('~/locales/en.json'),
      zh: require('~/locales/zh.json')
    }
  })
}
12345678910111213
```

## 3. 国际化保存至vuex:

store/locale.js:

```javascript
export default {
  state: () => ({
    locales: ['zh', 'en'],
    locale: 'zh'    
  }),
  mutations: {
    SET_LANG(state, locale) {  //用于更改当前国际化
      if (state.locales.includes(locale)) {
        state.locale = locale
      }
    }
  }
}
12345678910111213
```

## 4. 配置路由中间件：

middleware/i18n.js:

```javascript
export default ({ isHMR, app, store, error }) => {
  if ( isHMR ) return  //是否是通过模块热替换 webpack hot module replacement (仅在客户端以 dev 模式)
  const locale = app.$cookies.get('lang') || app.i18n.fallbackLocale
  if (!store.state.locale.locales.includes(locale)) {
    return error({ message: 'This page could not be found.', statusCode: 404 })
  }
  store.commit('locale/SET_LANG', locale)
  app.i18n.locale = locale
}
123456789
```

## 5. 配置到路由中间件内：

nuxt.config.js:

```javascript
export default {
  ...
  router: {  //路由配置
    middleware: ['i18n']  //路由中间件
  },
  plugins: [
    '~/plugins/i18n.js',  //添加至插件
  ],
}
123456789
```

## 6.vue使用国际化i18n:

```javascript
//页面使用：  (若：item.name = 'home'， 则：'<div>首页</div>')
<div>{{ $t('topbar.'+item.name) }}</div>

// 切换语言
computed: {
  ...mapState('locale', [ 'locale' ]),
},
methods: {
  ...mapMutations({ 'SET_LANG': 'locale/SET_LANG' }),
  switchLocale() {
    let locale = this.locale === 'zh' ? 'en' : 'zh'
    this.$i18n.locale = locale
    this.SET_LANG(locale)   //or this.$store.commit('locale/SET_LANG', locale)
    this.$cookies.set('lang', locale)
  }
}
```

### 中间件

```javascript
import getCookie from '@/config/get-cookie'
 
export default function ({store, route, redirect, req}) {
  const {lang} = getCookie(req)
  if (lang) {
    store.commit('setLang', lang)
  }
  const routePath = route.path
  if (store.state.lang === 'en' && routePath.indexOf(`/${store.state.lang}/`) === -1) {
    console.log('redirect')
    return redirect({path: `/${store.state.lang}${routePath}`, query: route.query})
  }
}
```

https://github.com/liuzhumin/nuxt-demo.git

