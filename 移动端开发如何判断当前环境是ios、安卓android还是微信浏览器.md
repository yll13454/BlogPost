判断当前环境是`ios`、`android`还是微信，我们可以从两个值进行判断。第一个参数值是`userAgent`，`window.navigator.userAgent`，用户代理，使得服务器能够识别客户使用的操作系统及版本、`CPU`类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等等。第二个参数值是`appVersion`，`window.navigator.appVersion`，可以返回浏览器的平台和版本信息。

**1. 移动端简单判断代码实现：**

```js
let ua = window.navigator.userAgent,
   app = window.navigator.appVersion;
   alert('浏览器版本: ' + app + '\n' + '用户代理: ' + ua);
   if(!!ua.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/)){
       // ios端 
       console.log('ios端');
   }
   else if(ua.indexOf('Android') > -1 || ua.indexOf('Adr') > -1) {
       // android端 
       console.log('android端');
   }
   if (ua.match(/MicroMessenger/i) == 'MicroMessenger') {
       // 微信浏览器 
       console.log('微信浏览器');
   }
```

**2. 移动端详细判断代码实现：**

```js
 var browser = {
        versions: function() {
          var u = navigator.userAgent, app = navigator.appVersion;
          return {
            trident: u.indexOf('Trident') > -1, //IE内核
            presto: u.indexOf('Presto') > -1, //opera内核
            webKit: u.indexOf('AppleWebKit') > -1, //苹果、谷歌内核
            gecko: u.indexOf('Gecko') > -1 && u.indexOf('KHTML') == -1, //火狐内核
            mobile: !!u.match(/AppleWebKit.*Mobile.*/) || !!u.match(/AppleWebKit/), //是否为移动终端
            ios: !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/), //ios终端
            android: u.indexOf('Android') > -1 || u.indexOf('Linux') > -1, //android终端或者uc浏览器
            iPhone: u.indexOf('iPhone') > -1 || u.indexOf('Mac') > -1, //是否为iPhone或者QQHD浏览器
            iPad: u.indexOf('iPad') > -1, //是否iPad
            webApp: u.indexOf('Safari') == -1 //是否web应该程序，没有头部与底部
          };
        }(),
        language: (navigator.browserLanguage || navigator.language).toLowerCase()
      }
      

   if (browser.versions.ios || browser.versions.iPhone || browser.versions.iPad) {
       //如果是ios系统就执行
       console.log('这是ios系统');
      }
   else if (browser.versions.android) {
        //如果是android系统就执行
        console.log('这是android系统');
      }
      
    var ua = navigator.userAgent.toLowerCase();
    if(ua.match(/MicroMessenger/i)=="micromessenger") {
         console.log('这是微信浏览器')
    } else {
        console.log('这不是微信浏览器');
        }
    }
```

**3. PC端与移动端的判断：**

```js
if (/(iPhone|iPad|iPod|iOS|Android)/i.test(navigator.userAgent)) { 
    //移动端
     console.log("这是移动端");
}else{
    //pc端
     console.log("这是pc端");
}
```