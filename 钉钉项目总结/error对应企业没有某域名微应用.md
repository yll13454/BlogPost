# [钉钉jsapi免登获取code中，出现对应企业没有某域名微应用](https://www.cnblogs.com/syscal/p/10304346.html)

在使用jsapi中。出现

{"errorMessage":"对应企业没有某域名微应用",:"errorCode":"3"}

```
window.onload = ``function``() {``  ``var` `ua = window.navigator.userAgent.toLowerCase();``  ``if` `(ua.match(/DingTalk/i) == ``'dingtalk'``) {``    ``dd.ready(``function``() {``      ``dd.runtime.permission.requestAuthCode({``        ``corpId: ``"xxxxxxx"``,``        ``onSuccess: ``function``(result) {``            ``window.location = ``"/dingtalk/auto?code="` `+ result.code;``        ``},``        ``onFail: ``function``(err) {``          ``alert(``"系统出错,请刷新重试."``);``        ``}``      ``});``    ``});``  ``} ``else` `{``    ``window.location = ``'/'``;``  ``}``}
```

　这是以上得到的code的代码。

**解决方法:**

**1.可能是你的企业没有建立微应用(就是你的工作台需要建自应用,还要添加授权域名)**

**2.你的corpId写错了。**

 

**最后： ** **域名可以是本地内网穿透**

 

