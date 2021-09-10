## 使用rollup-plugin-external-globals插件来解决问题 

参考：https://github.com/rollup/rollup/issues/2374

插件地址：https://www.npmjs.com/package/rollup-plugin-external-globals

### 安装插件 

Bash

```bash
yarn add -D rollup-plugin-external-globals
```

### 添加配置 

使用方法与上方定义方法几乎相同，传入参数给插件初始化方法就行。

JavaScript

```javascript
      plugins: [
        commonjs(),
        externalGlobals({
          vue: "Vue",
          "ant-design-vue": "antd",
          moment: "moment",
        }),
      ],
```

参数对解释：

- vue - 这里需要和external对应，这个字符串就是(import xxx from aaa)中的aaa，也就是包的名字
- Vue - 这个是js文件导出的全局变量的名字，比如说vue就是Vue，查看源码或者参考作者文档可以获得

下面是全部vite.config.js，供参考：

JavaScript

```javascript
import vue from "@vitejs/plugin-vue";
import commonjs from "rollup-plugin-commonjs";
import externalGlobals from "rollup-plugin-external-globals";

/**
 * https://vitejs.dev/config/
 * @type {import('vite').UserConfig}
 */
export default {
  plugins: [vue()],
  build: {
    rollupOptions: {
      external: ["vue", "ant-design-vue", "moment"],
      plugins: [
        commonjs(),
        externalGlobals({
          vue: "Vue",
          "ant-design-vue": "antd",
          moment: "moment",
        }),
      ],
    },
  },
};
```

### 在index.html中导入静态文件

修改根目录下的index.html，添加cdn文件：

Markup

```markup
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.min.css">
  <script src="https://cdn.jsdelivr.net/npm/vue@3.0.5/dist/vue.global.prod.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/moment@2.29.1/moment.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.js"></script>
```

整个文件参考下方：

Markup

```markup
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <link rel="icon" href="/favicon.ico" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.min.css">
  <script src="https://cdn.jsdelivr.net/npm/vue@3.0.5/dist/vue.global.prod.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/moment@2.29.1/moment.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/ant-design-vue@2.0.0-rc.9/dist/antd.js"></script>
  <title>Vite App</title>
</head>

<body>
  <div id="app"></div>
  <script type="module" src="/src/main.js"></script>
</body>

</html>
```