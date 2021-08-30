# [vite.config.ts基础配置分享](https://www.cnblogs.com/Man-Dream-Necessary/p/13725049.html)

更多配置参考：[https://vitejs.dev](https://vitejs.dev/)[
](https://github.com/vitejs/vite/blob/master/src/node/config.ts)

vite.config.ts

```
const fs = require("fs")
const path = require("path")

// Dotenv 是一个零依赖的模块，它能将环境变量中的变量从 .env 文件加载到 process.env 中
const dotenv = require("dotenv")

const envFiles = [
  /** default file */ `.env`,
  /** mode file */ `.env.${process.env.NODE_ENV}`
]

for (const file of envFiles) {
  const envConfig = dotenv.parse(fs.readFileSync(file))
  for (const k in envConfig) {
    process.env[k] = envConfig[k]
  }
}

module.exports = {
  assetsDir: './',
  alias: {
    '/@/': path.resolve(__dirname, './src')
  },
  optimizeDeps: {
    include: ['echarts']
  },
  hostname: process.env.VITE_HOST,
  port: process.env.VITE_PORT,
  // 引用全局 scss
  cssPreprocessOptions: {
    scss: {
      additionalData: '@import "./src/assets/style/index.scss";'
    }
  },
  // 压缩
  minify: 'esbuild',
  // 是否自动在浏览器打开
  open: false,
  // 是否开启 https
  https: false,
  // 服务端渲染
  ssr: false,
  /**
   * Base public path when served in production.
   * @default '/'
   */
  base: process.env.VITE_BASE_URL,
  /**
   * Directory relative from `root` where build output will be placed. If the
   * directory exists, it will be removed before the build.
   * @default 'dist'
   */
  outDir: process.env.VITE_OUTPUT_DIR,
  // 反向代理
  proxy: {
    api: {
      target: "http://www.skillnull.com",
      changeOrigin: true,
      rewrite: path => path.replace(/^\/api/, "")
    }
  }
}
```

　　

.env

```
# loaded in all cases``VITE_HOST = ``'0.0.0.0'``VITE_PORT = ``8080``VITE_BASE_URL = ``'./'``VITE_OUTPUT_DIR = ``'dist'
```

　　

.env.development

```
# 开发环境``VITE_API_DOMAIN = ``'/api'
```

　　

.env.production

```
# 生产环境``VITE_API_DOMAIN = ``'production.xxx.xxx'
```

　　

package.json

```
"scripts"``: {`` ``"dev"``: ``"NODE_ENV=development vite"``,`` ``"build-dev"``: ``"NODE_ENV=development vite build"``,`` ``"build-prd"``: ``"NODE_ENV=production vite build"``,`` ``"lint"``: ``"lint-staged ."``}
```

　　