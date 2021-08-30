## [javascript页面刷新的几种方法](https://www.cnblogs.com/xiaoqi2018/p/10826553.html)

**window.location.reload()**

**window.history.go(0)**

**document.execCommand(’‘Refresh’’)**

这三个方法是最快速的。其他的都有明显的浏览器滚动条的出现。

## Javascript刷新页面的几种方法：

#### 1 history.go(0)

除非有<%…%>等需在服务端解释才能生成的页面代码,否则直接读取缓存中的数据
不刷新

#### 2 location.reload()

要重新连服务器以读得新的页面(虽然页面是一样的)
刷新

#### 3 location=location

要在javascript中导航，不是调用window对象的某个方法，而是设置它的location.href属性，location属性是每个浏览器都支持的。比如：
<span οnclick=”javascript:window.location.href=’#top’”>top
执行后有后退、前进

#### 4 location.assign(location)

加载 URL 指定的新的 HTML 文档。 就相当于一个链接，跳转到指定的url，当前页面会转为新页面内容，可以点击后退返回上一个页面。

#### 5 document.execCommand(‘Refresh’)

#### 6 window.navigate(location)

MSDN说的window.navigate(sURL)方法是针对IE的，不适用于FF，在HTML DOM Window Object中，根本没有列出window.navigate方法。

#### 7 location.replace(location)

执行后无后退、前进
通过加载 URL 指定的文档来替换当前文档 ，这个方法是替换当前窗口页面，前后两个页面共用一个
窗口，所以是没有后退返回上一页的

#### 8 document.URL=location.href

### Javascript刷新页面的几种方法：

1 history.go(0)
2 location.reload()
3 location=location
4 location.assign(location)
5 document.execCommand(‘Refresh’)
6 window.navigate(location)
7 location.replace(location)
8 document.URL=location.href

自动刷新页面的方法: javascript自动刷新页面方法详解
1.页面自动刷新：把如下代码加入区域中

其中20指每隔20秒刷新一次页面.
2.页面自动跳转：把如下代码加入区域中

其中20指隔20秒后跳转到http://www.jbxue.com页面
3.页面自动刷新js版

```
<script language="JavaScript">
function myrefresh()
{
window.location.reload();
}
setTimeout('myrefresh()',1000); //指定1秒刷新一次
</script>
```

JS刷新框架的脚本语句

```
//如何刷新包含该框架的页面用 
<script language=JavaScript>
parent.location.reload();
</script>

//子窗口刷新父窗口
<script language=JavaScript>
self.opener.location.reload();
</script> www.jbxue.com
(　或　<a href="javascript:opener.location.reload()">刷新</a> )

//如何刷新另一个框架的页面用 
<script language=JavaScript>
parent.另一FrameID.location.reload();
</script>
```

如果想关闭窗口时刷新或者想开窗时刷新的话，在中调用以下语句即可。

```
<body onload="opener.location.reload()"> 开窗时刷新
<body onUnload="opener.location.reload()"> 关闭时刷新

<script language="javascript">
window.opener.document.location.reload()
</script>
```

