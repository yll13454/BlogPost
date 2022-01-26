根据页面 url 有选择地禁用 SSR 或 SPA 模式。

##  Nuxt 源代码

在详细介绍如何有选择地启用 SSR 之前，我们应该查看 Nuxt.js 源代码以了解 Nuxt 如何使这种区分成为可能。负责的代码段可以在[vue-renderer 包中找到](https://github.com/nuxt/nuxt.js/blob/99614535b5b4af0e2644ed172d7517804aaa1094/packages/vue-renderer/src/renderer.js#L311-L338)。让我们一行一行地剖析它！

1. 从上下文对象中提取`req`和`res`（从请求到服务器的请求和响应对象）。

```js
const { req, res } = context
```

1. 如果`context.spa`（可能在渲染页面之前通过其他内部设置）或`res.spa`（可以以其他方式修改！）为真，则将要渲染的页面视为**SPA**。

```js
const spa = context.spa || (res && res.spa)
```

1. 如果 SSR 被*禁用*或页面*应被视为 SPA*，则仅加载元数据并将页面呈现为 SPA（包含 JavaScript 文件，但不包含 HTML），通过*提前返回*。

```js
if (!this.SSR || spa) {
  const {/* ... */} = await this.renderer.spa.render(context)
  const html = this.renderTemplate(/* ... */)
  return { html, getPreloadFiles: this.getPreloadFiles.bind(this, { getPreloadFiles }) }
}
```

##  执行

在查看源代码后，我们发现我们所要做的就是设置`res.spa`要**禁用服务器端渲染的页面**。这只会发生在服务器端，因为 Nuxt 应用程序无论如何都会像客户端的传统 SPA 一样。如果我们只考虑服务器端操作，那么我们首先想到的应该是`serverMiddleware`（另请参阅： [通过服务器中间件通过 Nuxt.js 发送电子邮件](https://blog.lichter.io/posts/sending-emails-through-nuxtjs)）。使用它们有两个主要好处：

1. `serverMiddleware` 是通过框架直接提供的概念（不需要第 3 方库）
2. 我们可以自由定制我们的选择逻辑。

一个简约的实现看起来像这样：

```js
export default function(req, res, next) {
  const paths = ['/', '/a']

  if (paths.includes(req.originalUrl)) {
    // Will trigger the "traditional SPA mode"
    res.spa = true
  }
  // Don't forget to call next in all cases!
  // Otherwise, your app will be stuck forever :|
  next()
}
```

要直接查看工作示例，您可以查看下面的 CodeSandbox。请注意，它在`dev`模式下运行，因此 SPA 和 SSR 之间的差异不是很大，但仍然可以通过`process.server`.



 

**我是否需要有选择地在模式之间切换？**大多数时候答案是：**不**。但是在某些情况下，您可能希望避免将大量页面内容包装在`<ClientOnly>`作为 SEO 密集型应用程序一部分的标签（尤其是仪表板或会员区）中。