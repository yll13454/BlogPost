从父原则

dom结构：

```html
<div class="a">
    <div class="b"></div>
</div>
<div class="c"></div>
```



chorm22+中，当设置position:fixed;后会触发元素创建一个新的层叠上下文，并且当成一个整体在父层叠上下文中进行比较。如上面的dom结构，当给b设置了position:fixed;属性后，会触发创建一个新的层叠上下文，它的父层叠上下文变成了a，所以b只能在a的内部进行层叠比较。这也就是大家熟听的“从父原则”。

其他浏览器在进行position:fixed;设置时，不会触发层叠上下问的重新创建，因此，整个页面只有root 和
b两个层叠上下文，所有元素都在root层叠上下文进行比较。

so，请注意下面两点：
1. z-index只有在设置了position为relative,absolute,fixed时才会有效。
2. z-index的“从父原则”。当你发现把z-index设的多大都没用时，看看其父元素是否设置了有效的z-index，当然别忘了先看看自身是否设置了position。
————————————————
版权声明：本文为CSDN博主「尼古拉斯-托尔斯泰-赵四」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_31411389/article/details/78116419



[不受控制的 position:fixed](https://www.cnblogs.com/coco1s/p/7358830.html)

