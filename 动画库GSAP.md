GSAP全称是GreenSock Animation Platform包括以下代码：

- TweenLite：

  是一个极其快、轻量级、灵活的动画工具，为GSAP的底层服务。

- TweenMax：

  是TweenLite的扩展，添加了很多有用的功能如：repeat(), repeatDelay(), yoyo()更多，还默认包括了一些有用的插件。
  任意的插件也能在TweenLite上运行。TweenMax免去了你加载一些插件，如： CSSPlugin, RoundPropsPlugin, BezierPlugin, AttrPlugin, DirectionalRotationPlugin，EasePack, TimelineLite, and TimelineMax。TweenMax和TweenLite语法完全一样。

- TimelineLite：

  是一个强大的排序工具。官方解释的抽象，用我自己的话来说就是一个链式工具，如jquery一样写法都是可以这样写，$(‘dom’).width().css().eq().animate。
  这工具就是起到这样的作用，也有点类似队列动画的效果，上一个动画执行完成才能到下一个动画执行。避免了还要去一个个计算delay的时间才实现队列动画效果。TimelineLite也有自己的很多方法。

- TimelineMax：

  是TimelineLite的扩展，又在TimelineLite基础上添加了很多有用的方法。[TimelineMax文档](https://jskoa.com/?p=5075)

代码概况：

```js
var mov=new TweenMax.to(
  '#box', //操作对象
  2, //动画持续时间
  {
    css:{
      left:100,
      opacity:0.3
    },
    startAt:{left:10}, //初始化
    delay:0, //1秒后开始执行动画
    ease:Back.easeOut, //加速度效果
    repeat:0, //多循环1次，也就是一共重复执行了2次同样的动画，-1就会无限重复循环
    repeatDelay:0, //每次重复循环时间3秒,需要配合repeat属性
    yoyo:false, //如果true，动画的循环是倒序，需要配合repeat属性
    paused:false, //如果true,动画暂停
    overwrite:'auto',
    onStart:function(){ //动画开始前回调
       document.title='开始前执行'
    },
    onStartParams:["{self}", "参数"],
    onComplete:function(a,b){ //动画结束后回调
       document.title='动画完成'
    },
    onCompleteParams:["{self}", "param2"], //为onComplete传参数，其中{self}值的是mov
    immediateRender:true,
    onUpdate:function(a,b){ //初始化执行一次，动画过程回调，以后动画有运动都还会不停的执行，直到动画结束才停止
      // console.log(a.isActive());
    },
    onUpdateParams:["{self}", "参数"], //为onUpdate提供参数
    useFrames:false
 });
```

链式写法：GASP的链式写法要靠TimelineLite来实现的，如：

```js
var tl = new TimelineMax({repeat:-1, delay:0, repeatDelay:2}); //公共属性可以写这里
tl.to('#box',10,{left:100,borderColor:'red'})  //独立属性可以各自写
  .to('#box',2,{top:100,borderColor:'yellow'});

//或者

//推荐这样写，直观点
var tl = new TimelineMax({repeat:2, repeatDelay:1});
    tl.add( TweenMax.to(element, 1, {left:100}) );
    tl.add( TweenMax.to(element, 1, {top:50}) );
    tl.add( TweenMax.to(element, 1, {opacity:0}) );
```

批量操作动画

```js
TweenMax.staggerFrom('.box', 5, {y:"150"}, 5); //动画逆播，获取全部class为box的元素，动画时间5秒，从y是150的位置开始到0,每个元素之间间隔5秒
TweenMax.staggerTo('.box', 1, {y:"+=150", opacity:0}, 0.2); //按顺序播放
TweenMax.staggerFromTo('.box', 5, {y:"150",x:'100'},{y:"00",x:'0'}, 5); //自行设置开始结束播放

TweenMax.staggerFrom('.box', 2, {
  cycle:{
    y:[50,-50], //第一个元素从50开始，第二个元素从-50开始，以此类推
    y:function(i){
       console.log(this); //this是dom元素本身
       return i * 20; //i是当前元素的索引
    }
 }
},2);
```

 

属性链式写法

```js
 box.repeat(2).yoyo(true).repeatDelay(0.5).play();
```

 

延迟回调，指定几秒后才发生回调

```js
TweenMax.delayedCall(3, myFunction, [true, "param2"]);

function myFunction(param1, param2) {
 console.log(param1, param2);
}
```

 

```js
myAnimation.duration() //获取动画运行的时间
myAnimation.duration(10) //重新设置动画运行的时间

myAnimation.timeScale(2) //速度的设置，2倍速度，效果相当于把duration的时间改小是一样的

myAnimation.endTime() //提前获取动画运动消耗的时间

mov.eventCallback("onComplete", myFunction, ["param1","param2"])
function myFunction(param1, param2) {
 document.title=111;
}

myAnimation.isActive() //是否在动画中

TweenMax.globalTimeScale() //获取当前正常速度
TweenMax.globalTimeScale(0.5) //设置全局速度是原来速度慢一半
TweenMax.globalTimeScale(2) //比原来速度快2倍

mov.kill({left:true}); //禁用动画某一个种过渡属性

TweenMax.killAll(是否强制完成,是否禁用动画,是否禁用delayedCalls回调,是否禁用timelines);
TweenMax.killAll(true,false,false,false); //默认值都为false,如果第一个参数值为true,意思就是强制他们一定要完成效果或者是回调
TweenMax.killAll(false,true,false,false); //所有过渡动画都停止
TweenMax.killAll(true, true, false, false); //强制动画瞬间结束，并且保留动画结束的时的效果

TweenMax.killChildTweensOf( document.getElementById("fa") ); //父元素fa下的所有元素都禁止运动

mov.pause(6,true); //暂停在第6秒时候的动画,并且禁用任何回调
mov.paused(false); //恢复暂停
mov.paused(true); //暂停
mov.paused(); //获取动画是否暂停

TweenMax.pauseAll(tweens,delayedCalls,timelines);

mov.play(10,true); //从10秒开始动画,并且禁用任何回调
mov.progress( 0.5 ,true); //从进度的百分之50%后开始动画,并且禁用任何回调

 .repeat() //循环
 .repeatDelay() //循环间隔时间

mov.restart(true); //包括delay时间也重置
mov.restart(false); //忽略delay时间，动画立马执行

mov.resume(2); //重置，从2秒时刻开始

mov.reverse(0); //动画从结束状态开始倒退回开始状态
mov.reverse(-10); //动画从倒数第10秒开始逆播
mov.reversed(false); //禁止动画逆播

mov.startTime(2); //过几秒后开始动画，类似于delay属性优先于delay
mov.time(5); //从第5秒开始动画,忽略delay属性
mov.totalDuration(2); //动画持续时间
```

更多API自行去官网看

全部GASP插件集合打包：https://github.com/thealrivera/GSAP_UI

百度下载： http://pan.baidu.com/s/1nv9zU7v

国内各个版本下载：http://www.bootcdn.cn/gsap/

本地下载：[GSAP_UI-master](https://jskoa.com/wp-content/uploads/2016/05/GSAP_UI-master.zip)

如果想去官网下载使用，有些功能是需要注册会员收费的！！

# [动画库GSAP 文本替换动画TextPlugin.min.js](https://jskoa.com/?p=5071)

代码风格

```js
TweenLite.to('#box1', 2, {
  text:{
    value:"This is the new text",  //要显示的文本
    delimiter:" ", //以空格为单位，一个单词显示，而不是一个字母逐步显示
    padSpace:true, //新文本比旧文本过短时吗，建议开启
    newClass:"class2", //新文本容器的class名
    oldClass:"class1"  //旧文本容器的class名
  },
 ease:Linear.easeNone //速度效果
});
```

# [动画库GSAP 字符串处理插件SplitText.min.js](https://jskoa.com/?p=5066)



SplitText.min.js会将一段字符串不管是字母还是中文，会按照需求分割并用div包裹着。

SplitText.min.js是动画库GSAP提供的一款插件，但是它可以独立使用，并且还是收费的！！

[破解版SplitText.min.js下载](https://jskoa.com/wp-content/uploads/2016/06/SplitText.min_.js)

 

代码：

```js
var split = new SplitText("#yourElementID", //选择器
{
  type:"chars,words,lines", //以单个字母分割，以单词分割，以行分割
  position:'absolute',
  charsClass:'aa++', //批量添加class,如aa1,aa2,aa3,...
  linesClass:'line++',
  wordsClass:'word++'
});
```

 

选择器：

```js
var yourElement = document.getElementById("yourID");
var split = new SplitText(yourElement); //原生DOM方式
 
var split = new SplitText("#yourID"); //模仿jquery

var split = new SplitText( $(".yourClass") ); //利用jquery获取
 
var split = new SplitText([element1, element2, element3]);
```

# [GASP | TimelineMax时间轴](https://jskoa.com/?p=5075)

```js
TimelineMax
.add() //在时间末尾添加一个动画、标签、回调
//add a tween to the end of the timeline
 tl.add( TweenLite.to(element, 2, {left:100}) );
 
 //add a callback at 1.5 seconds
 tl.add(func, 1.5); 
 
 //add a label 2 seconds after the end of the timeline (with a gap of 2 seconds)
 tl.add("myLabel", "+=2");
 
 //add another timeline at "myLabel"
 tl.add(otherTimeline, "myLabel"); 
 
 //add an array of tweens 2 seconds after "myLabel"
 tl.add([tween1, tween2, tween3], "myLabel+=2"); 
 
 //add an array of tweens so that they are sequenced one-after-the-other with 0.5 seconds inbetween them, starting 2 seconds after the end of the timeline
 tl.add([tween1, tween2, tween3], "+=2", "sequence", 0.5);
```

 

.addPause() //添加一个暂停，能在指定时间后暂停并回调

```js
.addPause(10, function(a,b){console.log(a,b)}, ["param1", "param2"], this); //在10秒后暂停并回调
```

addCallback() //功能用法、参数跟addPause()一样只是缺少了暂停功能

 

set() //立刻生效，不进行过渡效果

```js
.set('#box', {opacity:0.5})
```

