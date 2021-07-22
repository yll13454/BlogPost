**className** 获取或设置指定元素的class属性的值。

## [语法](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/className#syntax)

```
let cName = elementNodeReference.className;

elementNodeReference.className = cName;
```

- cName是一个字符串变量,表示当前元素的`class`属性的值,可以是由空格分隔的多个`class`属性值.

## [示例](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/className#example)

```
let elm = document.getElementById("div1");

if (elm.className == "fixed") {
  // 跳过class属性为特定值的元素
  goNextElement();
}
```

Copy to Clipboard

## [注释](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/className#notes)

使用名称`className`而不是`class`作为属性名,是因为"class" 在JavaScript中是个保留字.

