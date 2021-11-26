### inspector tab (检查器选项卡)

我们可以通过检查器查看每个组件的状态，这个检查器就是罗盘状的图标。

![img](media/1baf8109935841a4be74b2f40499f205tplv-k3u1fbpfcp-watermark.awebp)

![img](media/083fa9cc1f9d4760a9439c475689780ftplv-k3u1fbpfcp-watermark.awebp)

### 组件操作图标

当选择一个组件时，会看到右上方有一组三个不同的图标。

![img](media/51f06f2e8af449f289eb22d481d87928tplv-k3u1fbpfcp-watermark.awebp)

#### 第一个眼睛的图标的表示 `Scroll to component`。当点击这个图标时，浏览器将会滚动到组件所在的位置。

![img](media/47cc5bc4873742779cb0beb057d34496tplv-k3u1fbpfcp-watermark.awebp)

#### 第二个 `<>` 表示的是 `Show render code`。当点击这个图标时，可以看到当前组件的`Render`函数。

![img](media/9812824130364cb4a1b9aad04549d847tplv-k3u1fbpfcp-watermark.awebp)

#### 最后，带有`<`的汉堡包图标表示检查DOM。点击它时，就会显示组件也表示 Dom 的位置。

![img](media/bbc57a29855e4926837be2eadb198afetplv-k3u1fbpfcp-watermark.awebp)

### 多根组件

你可能已经注意到了，在我们组件旁边有些小标签，如下所示：

![img](media/58a45445bb6a4d19bb087a0c58f3ac89tplv-k3u1fbpfcp-watermark.awebp)

首先可以看到有 `fragment` 标记，它表示多根组件，啥是多根，直接看我们`FragmentComponent.vue` 的内容：

```
<template>
  <div>Fragment1</div>
  <div>Fragment2</div>
</template>
复制代码
```

多根就是没有像 Vue2 一样，只有一个根元素，不能多个。

### 性能指示

除了多根组件的标识，我们还可以看到一些数字的标识：

![img](media/f3c7d677feaf45a29fd461956785a47etplv-k3u1fbpfcp-watermark.awebp)

当我们的组件因为其渲染速度慢而表现不佳时，它就会显示出来，告诉我们哪些组件耗时比较严重。

如上图所示，当你把鼠标悬停在它上面时，可以看到有更多信息提示。

![img](media/743be65ce55943fb976f87ffb4a2f8c8tplv-k3u1fbpfcp-watermark.awebp)

### 路由指示器

除了多根和性能指示器外，还有一个路由指示器：

![img](media/5999fd55aba545de975881166de31a67tplv-k3u1fbpfcp-watermark.awebp)

这个新特性在快速查看 links 的设置很方便。但奇怪的是，这个特性并不是由 Vue tools 本身直接添加的，而是由Vue Router 添加的.

