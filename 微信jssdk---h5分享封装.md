# 微信jssdk---h5分享封装

```js
import { getWxConfig } from 'api/index/index.js';
import { wxPay } from 'api/index/index.js';
import wx from 'weixin-js-sdk'
export default {
    //判断是否在微信中  
    isWechat: function() {
        var ua = window.navigator.userAgent.toLowerCase();
        if (ua.match(/micromessenger/i) == 'micromessenger') {
            // console.log('是微信客户端')
            return true;
        } else {
            // console.log('不是微信客户端')
            return false;
        }
    },
    //初始化sdk配置  这个上篇文章有详细讲解，就不赘述了
    initJssdk: function(callback, url) {
        console.log(url)
        //服务端进行签名
        let data = {
            url: encodeURIComponent(url)
        }
        getWxConfig(data).then(res => {
            if (res.status == 1000) {
                if (res.data) {
                    wx.config({
                        debug: false,
                        appId: res.data.appId,
                        timestamp: res.data.timestamp,
                        nonceStr: res.data.nonceStr,
                        signature: res.data.signature,
                        //这里把分享的apilist里加上分享的接口名称
                        jsApiList: [
                            'checkJsApi',
                            'onMenuShareTimeline',
                            'onMenuShareAppMessage',
                            'updateAppMessageShareData',
                            'updateTimelineShareData'
                        ]
                    });
                    //配置完成后，再执行分享等功能  
                    if (callback) {
                        callback(res.data);
                    }
                }
            }
        }).catch(err => {})
    },

    //this.$wechat.share(data,url)封装后调用
    //在需要自定义分享的页面中调用  
    share: function(data, url) {
        //data是我们传递过来的需要分享的内容。
        //url是当前页面路径
        url = url ? url : window.location.href.split('#')[0];
        //每次都需要重新初始化配置，才可以进行分享  
        this.initJssdk(function(callback) {
            wx.ready(function() {
                var shareObject = {
                    title: data.title,//// 分享标题
                    desc: data.des,//// 分享描述
                    link: data.url,//分享链接，该链接域名或路径必须与当前页面对应的公众号JS安全域名一致
                    imgUrl: data.img,//// 分享图标
                    success: function(res) {
                        //业务回调
                    },
                    cancel: function(res) {
                        this.$toast({
                            icon: 'none',
                            title: '分享失败!',
                            duration: 1000
                        })
                    }
                };
                //分享给朋友及分享到QQ
                wx.updateAppMessageShareData(shareObject);
                //分享到朋友圈及分享到QQ空间  
                wx.updateTimelineShareData(shareObject);
            });
        }, url);
    },

}
```

