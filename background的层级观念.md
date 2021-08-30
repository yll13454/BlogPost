`background`允许你向元素应用多个背景。

这些背景相互堆叠，第一个背景放在最上面，最后一个背景放在最下面。 仅最后一个背景允许拥有背景色。

也就是说些写在最前面的背景在最上面，只有最后一个背景可以加上背景色，可以理解为背景色的层级最低。

```
.bg{
  background: background1, background2, bgColor background3;
}
```

**background支持图片和颜色渐变**

```css
.bg{
  background-image: [background-image], [background-image], [background-image]; 
  background-position: [background-position], [background-position], [background-position]; 
  background-repeat: [background-repeat], [background-repeat], [background-repeat]; 
  background-size: [background-size], [background-size], [background-size];
}
```

总结一下就是分别依次写明`background`的相关属性，而`background-position`/`background-repeat`/`background-size`依次对应`background-image`的图片顺序。

当然，正常来说不用拆分得这么复杂，除了`background-size`比较傲娇，不肯和别人写一块之外，其他的属性都可以缩写在一起。

```
.bg{
  background: [background-image] [background-position] [background-repeat], 
              [background-image] [background-position] [background-repeat], 
              [background-image] [background-position] [background-repeat]; 
  background-size: [background-size], [background-size], [background-size];
}
```



在背景图片前面添加一个渐变初始值和结束值一样的渐变层，也就是说写一个假装是渐变的颜色层。

```
.bg{
  background: -webkit-linear-gradient(top, rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url("img1.png") 0 0 no-repeat;
}
```

1. background中渐变和图片可以作为一个单独的层，定义在最前面的层级最高；
2. 只有最后一个图片层可以带背景色；
3. 除了background-size之外，其他属性可以缩写在background中定义。