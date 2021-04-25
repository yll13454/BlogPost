# javascript-如何正确卸载/销毁VIDEO元素

我正在开发一个实时媒体浏览/播放应用程序，该应用程序在浏览器中使用29534209537930700700对象进行播放（如果有）。

我混合使用了直接的javascript和jQuery，

我特别关心记忆。 该应用程序永远不会在窗口中重新加载，并且用户可以观看许多视频，因此随着时间的推移，内存管理成为一个大问题。 在今天的测试中，我看到内存配置文件随着要加载的每个后续视频流的视频大小而跳跃，并且从未降落到基线。

我已经尝试了以下结果相同的事情：

1-清空包含已创建元素的父容器，例如：

```
$(container_selector).empty();
```



2-暂停并删除与“视频”匹配的子项，然后清空父容器：

```
$(container_selector).children().filter("video").each(function(){
    this.pause();
    $(this).remove();
});
$(container_selector).empty();
```



3-部署来自DOM结构的视频非常棘手。 这可能会导致浏览器崩溃。 这是对我的项目有帮助的解决方案。

```
var videoElement = document.getElementById('id_of_the_video_element_here');
videoElement.pause();
videoElement.removeAttribute('src'); // empty source
videoElement.load();
```

这将重置所有内容，保持无错误！

