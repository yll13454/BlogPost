![image-20210817122806323](media/image-20210817122806323.png)

```css
background: url('https://hxy-weapp.oss-cn-hangzhou.aliyuncs.com/vip-channel-manage/login/bg-l.png')
        0 75px/126px 323px no-repeat,
      url('https://hxy-weapp.oss-cn-hangzhou.aliyuncs.com/vip-channel-manage/login/bg-r.png') right
        23px/118px 195px no-repeat,
      url('https://hxy-weapp.oss-cn-hangzhou.aliyuncs.com/vip-channel-manage/login/bg-b.png') left
        bottom/70px 66px no-repeat,
      linear-gradient(133deg, #3266ed 0%, #5ea9ff 100%);
```

### background:图片地址，图片位置/图片尺寸 是否重复，linear渐变色;

```css
#divtest{
            width:400px;
            height:400px;
            background-color: aqua;
            background-image: url("img/2.jpg");
            /*background: url("img/2.jpg") 10% 20% no-repeat;!*图片从左往右移动的百分比大小，从上往下百分比大小，重复方式
                                                            换成数值同样如此，在这里没有调整大小的方法*!*/
            background-position: 10% 40%;/*这个是按从左往右，从上往下的百分比位置进行调整*/
            background-size: 50% 50%;/*按比例缩放*/
            /*background-size: 100px 100px;!*这个是按数值缩放*!*/
            background-repeat: no-repeat;/*还有repeat-x,y等*/
        }
```









如果您还想为图像设置***背景位置\***，则可以使用以下命令：

```css
background-color: #444; // fallback
background: url('PATH-TO-IMG') center center no-repeat; // fallback

background: url('PATH-TO-IMG') center center no-repeat, -moz-linear-gradient(top, @startColor, @endColor); // FF 3.6+
background: url('PATH-TO-IMG') center center no-repeat, -webkit-gradient(linear, 0 0, 0 100%, from(@startColor), to(@endColor)); // Safari 4+, Chrome 2+
background: url('PATH-TO-IMG') center center no-repeat, -webkit-linear-gradient(top, @startColor, @endColor); // Safari 5.1+, Chrome 10+
background: url('PATH-TO-IMG') center center no-repeat, -o-linear-gradient(top, @startColor, @endColor); // Opera 11.10
background: url('PATH-TO-IMG') center center no-repeat, linear-gradient(to bottom, @startColor, @endColor); // Standard, IE10
```

或者你也可以创建一个 LESS mixin（引导程序风格）：

```css
#gradient {
    .vertical-with-image(@startColor: #555, @endColor: #333, @image) {
        background-color: mix(@startColor, @endColor, 60%); // fallback
        background-image: @image; // fallback

        background: @image, -moz-linear-gradient(top, @startColor, @endColor); // FF 3.6+
        background: @image, -webkit-gradient(linear, 0 0, 0 100%, from(@startColor), to(@endColor)); // Safari 4+, Chrome 2+
        background: @image, -webkit-linear-gradient(top, @startColor, @endColor); // Safari 5.1+, Chrome 10+
        background: @image, -o-linear-gradient(top, @startColor, @endColor); // Opera 11.10
        background: @image, linear-gradient(to bottom, @startColor, @endColor); // Standard, IE10
    }
}
```



需要意识到的一件事是，第一个定义的背景图像位于堆栈的最顶部。最后定义的图像将位于最底部。这意味着，要在图像后面设置背景渐变，您需要：

```css
  body {
    background-image: url("http://www.skrenta.com/images/stackoverflow.jpg"), linear-gradient(red, yellow);
    background-image: url("http://www.skrenta.com/images/stackoverflow.jpg"), -webkit-gradient(linear, left top, left bottom, from(red), to(yellow));
    background-image: url("http://www.skrenta.com/images/stackoverflow.jpg"), -moz-linear-gradient(top, red, yellow);
  }
```

### https://stackoverflow.com/questions/2504071/how-do-i-combine-a-background-image-and-css3-gradient-on-the-same-element

