# 基于 Custom Elements 的组件化开发

[customElements](https://link.juejin.cn/?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FWeb_Components%2FUsing_custom_elements) 是 Web Components 规范下的新 API，可以用来实现组件化开发。

## 基本用法

组件声明在一个 HTML 文件中。组件包括样式（Style），节点（DOM）和交互逻辑（Script）。一个组件文件的基本结构如下：

```vue
<template>
  <style></style>
  <div>DOM 节点</div>
</template>
<script>
  const componentDocument = document.currentScript.ownerDocument;

  class Component extends HTMLElement {

    static get TAG_NAME() {
      return 'component-tag-name';
    };

    constructor() {
      super();
      const shadow = this.attachShadow({ mode: 'closed' });
      const content = componentDocument.querySelector('template').content.cloneNode(true);
      shadow.appendChild(content);
    }
  }

  customElements.define(Component.TAG_NAME, Component);
</script>
```

其中 `template` 节点下包含样式（Style）和节点（DOM）。交互逻辑在 `script` 标签中。

组件文件通过 `<link rel="import" href="./component.html">` 的方式引入 HTML 文件中。在 HTML 文件中使用组件的方式就是直接写组件标签。比如：

```html
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=0">
  <title>HTML</title>
  <link rel="import" href="./component.html">
</head>
<body>
<component-tag-name></component-tag-name>
</body>
</html>
```

## 组件注册

`customElements.define` API 用来组册组件，API 接受三个参数：组件标签名称、组件的类和组件继承的标签类型。如：

```
customElements.define('expanding-list', ExpandingList, { extends: "ul" });
```

上面声明了一个标签为 `expanding-list` 的组件，组件的构造类是 `ExpandingList` 需要声明，组件继承 `ul` 标签的特性。

## 组件构造类


作者：vivaxy
链接：https://juejin.cn/post/6844903950043332615
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# [腾讯发布前端组件框架 Omi，全面拥抱 Web Components](https://juejin.cn/post/6844903695067381767)

