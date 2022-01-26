首先**子元素会继承父元素的透明度**：

- 设置父元素opacity：0.5，子元素不设置opacity，子元素会受到父元素opacity的影响，也会有0.5的透明度。

其次**子元素的透明度是基于父元素的透明度计算的**：

- 设置父元素opacity：0.5，即使设置子元素opacity：1，子元素的opacity：1也是在父元素的opacity：0.5的基础上设置的，因此子元素的opacity还是0.5。

**解决办法**：

> 利用CSS3属性rgba（即red+green+blue+alpha的颜色），
>
> 例如background-color:rgba(0,0,0,0.5)
>
> 但是IE7/8不支持此属性，可按如下方法写：
>
> 父元素div要写如下：
>
> background-color: rgba(0,0,0,0.5)!important;
>
> background-color: #000;
>
> filter:Alpha(opacity=50);
>
> 子元素div加个定位position:absolute/relative即可。

## 伪元素

父类设置

opacity：0.5

伪类元素也会受到影响

**解决办法**：

>父类设置 background: rgba(255, 255, 255, 0.2);
>
>伪元素正常设置就不会受到影响。