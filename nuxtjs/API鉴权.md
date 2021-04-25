对于某些敏感的服务，我们可能需要对API进行鉴权，防止被人轻易盗用我们node端的API，因此我们需要做一个API的鉴权机制。常见的方案有jwt，可以参考一下阮老师的介绍：[《JSON Web Token 入门教程》](http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html)。如果场景比较简单，可以自行设计一下，这里提供一个思路：

1. 客户端和node端在环境变量中声明一个秘钥：SECRET=xxxx，注意这个是保密的;
2. 客户端发起请求时，将当前时间戳(timestamp)和`SECRET`通过某种算法，生成一个`signature`，请求时带上`timestamp`和`signature`；
3. node接收到请求，获得`timestamp`和`signature`，将`timestamp`和秘钥用同样的算法再生成一次签名`_signature`
4. 对比客户端请求的`signature`和node用同样的算法生成的`_signature`，如果一致就表示通过，否则鉴权失败。

具体的步骤：

客户端对axios进行一层封装：

```
import axios from 'axios'
import sha256 from 'crypto-js/sha256'
import Base64 from 'crypto-js/enc-base64'
// 加密算法，需安装crypto-js
function crypto (str) {
  const _sign = sha256(str)
  return encodeURIComponent(Base64.stringify(_sign))
}

const SECRET = process.env.SECRET

const options = {
  headers: { 'X-Requested-With': 'XMLHttpRequest' },
  timeout: 30000,
  baseURL: '/api'
}

// The server-side needs a full url to works
if (process.server) {
  options.baseURL = `http://${process.env.HOST || 'localhost'}:${process.env.PORT || 3000}/api`
  options.withCredentials = true
}

const instance = axios.create(options)
// 对axios的每一个请求都做一个处理，携带上签名和timestamp
instance.interceptors.request.use(
  config => {
    const timestamp = new Date().getTime()
    const param = `timestamp=${timestamp}&secret=${SECRET}`
    const sign = crypto(param)
    config.params = Object.assign({}, config.params, { timestamp, sign })
    return config
  }
)

export default instance
```

**server端**

接着，在server端写一个鉴权的中间件，`/server/middleware/verify.js`：

```
const sha256 = require('crypto-js/sha256')
const Base64 = require('crypto-js/enc-base64')

function crypto (str) {
  const _sign = sha256(str)
  return encodeURIComponent(Base64.stringify(_sign))
}
// 使用和客户端相同的一个秘钥
const SECRET = process.env.SECRET

function verifyMiddleware (req, res, next) {
  const { sign, timestamp } = req.query
  // 加密算法与请求时的一致
  const _sign = crypto(`timestamp=${timestamp}&secret=${SECRET}`)
  if (_sign === sign) {
    next()
  } else {
    res.status(401).send({
      message: 'invalid token'
    })
  }
}

module.exports = { verifyMiddleware }
复制代码
```

最后，在需要鉴权的路由中引用这个中间件, `/server/index.js`：

```
const { Router } = require('express')
const { verifyMiddleware } = require('../middleware/verify.js')
const router = Router()

// 在需要鉴权的路由加上
router.get('/test', verifyMiddleware, function (req, res, next) {
    res.json({name: 'test'})
})
```


作者：fengxianqi
链接：https://juejin.cn/post/6844903906200256525
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

**`encodeURIComponent()`**函数通过将一个，两个，三个或四个表示字符的UTF-8编码的转义序列替换某些字符的每个实例来编码 [URI](https://wiki.developer.mozilla.org/en-US/docs/Glossary/URI) （对于由两个“代理”字符组成的字符而言，将仅是四个转义序列） 。

```js
// encodes characters such as ?,=,/,&,:
console.log(`?x=${encodeURIComponent('test?')}`);
// expected output: "?x=test%3F"

console.log(`?x=${encodeURIComponent('шеллы')}`);
// expected output: "?x=%D1%88%D0%B5%D0%BB%D0%BB%D1%8B"
```

