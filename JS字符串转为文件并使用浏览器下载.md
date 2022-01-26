> 前端资源下载，可以使用HTML5原生的download 属性



```xml
<a href="test.jpg download="test.jpg" ></a> // 并且可以指定文件名
```

- 项目有个需求是后端接口传html字符串，需要转化为HTML文件并且下载。也可以用这种方式实现

- 实现方式：



```jsx
function (content, filename) {
  // 创建a标签
  var eleLink = document.creattElement('a')
  // 设置a标签 download 属性，以及文件名
  eleLink.download = filename
  // a标签不显示
  eleLink.style.display = 'none'
  // 获取字符内容，转为blob地址
  var blob = new Blob([content])
  // blob地址转为URL
  eleLink.href = URL.createObjectURL(blob)
  // a标签添加到body
  document.body.appendChild(eleLink)
  // 触发a标签点击事件，触发下载
  eleLink.click()
  // a标签从body移除
  document.body.removeChild(eleLink)
}
```

