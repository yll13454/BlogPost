#### 数据mock

安装依赖

```bash
npm i mockjs -S
npm i vite-plugin-mock cross-env -D
```

引入插件，`vite.config.js`

```js
import { viteMockServe } from 'vite-plugin-mock'
plugins: [
  viteMockServe({
      mockPath: 'mock',
      supportTs: false,
    }),
],
```

设置环境变量，`package.json`

```
"dev": "cross-env NODE_ENV=development vite"
```

创建`mock`文件，`mock/test.js`

```js
import { MockMethod } from 'vite-plugin-mock'
import Mock from 'mockjs'
const Random = Mock.Random
const code = 200

export default [
  {
    url: '/mock/list',
    method: 'get',
    response: ({ query }) => {
      const pageSize = Number(query.size || query.pageSize || 10)
      const datas = new Array(pageSize).fill(0).map(() =>
        Mock.mock({
          id: Random.id(),
          'status|1': ['ONLINE', 'OFFLINE'],
          title: Random.cword(4),
          url: Random.image(),
        })
      )
      return { code, data: datas }
    },
  },
  {
    url: '/mock/detail',
    method: 'get',
    response: ({ query }) => {
      const data = Mock.mock({
        id: Random.id(),
        'status|1': ['ONLINE', 'OFFLINE'],
        title: Random.cword(4),
        url: Random.image(),
      })
      return { code, data }
    },
  },
  {
    url: '/mock/status',
    method: 'get',
    response: ({ query }) => {
      const data = { message: '操作成功' }
      return { code, data }
    },
  },
  {
    url: '/mock/fail',
    method: 'get',
    response: ({ query }) => {
      const data = { message: '操作失败' }
      return { code: 500, data }
    },
  },
  {
    url: '/mock/res',
    method: 'get',
    response: ({ query }) => {
      const code = query.err ? 500 : 200
      const message = query.err || '操作成功'
      const data = { message }
      return { code, data }
    },
  },
] as MockMethod[]
```


作者：杨村长
链接：https://juejin.cn/post/6910014283707318279
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。