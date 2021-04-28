## H5适配处理iphoneX底部的横条

iphoneX手机取消了实体Home键，取而代之的是主界面底部不显眼的横条“Home Indicator”。当网页底部fixed 元素时候，一部分元素可能就被这个横条遮挡住，怎么适配解决呢？

**第一步：**<meta name=“viewport” content=“width=device-width, viewport-fit=cover”>

**第二步：**页面主体内容限定在安全区域内,如果不设置这个值，可能存在小黑条遮挡页面最底部内容的情况

　　　　body { padding-bottom: env(safe-area-inset-bottom); }

**第三步：**

fixed 元素的适配：**给fixed元素添加以下属性**

- 第一种：padding-bottom: env(safe-area-inset-bottom); 注意元素是否设置box-sizing:border-box;否则不起作用，道理就不说了。
- 第二种：height: calc(60px(假设值) + env(safe-area-inset-bottom));
- 第三种：margin-bottom: env(safe-area-inset-bottom);

这三种方式都可解决，视情况而定选择合适的，也可以灵活为fixed 元素的子元素添加这些属性

###### fixed 非完全吸底元素（bottom ≠ 0），比如 “返回顶部”、“侧边广告” 等

给该元素设置

```css
.fixDiv{ 
bottom: calc(50px + env(safe-area-inset-bottom)) !important; 
height: calc(50px - env(safe-area-inset-bottom)) !important; /*50px为假设值*/ 
} 
```

##### css 函数 env() 和 constant()

上面两个函数可以直接使用变量函数，只有在 webkit 内核下才支持 env() 必须在 ios >= 11.2 才支持 constant() 必须 ios < 11.2 支持 env 和 constant 只有在 viewport-fit=cover 时候才能生效 兼容前后版本,例子：

```css
body {
padding-bottom: env(safe-area-inset-bottom);
padding-bottom: constant(safe-area-inset-bottom);
}  
```

参考：

https://aotu.io/notes/2017/11/27/iphonex/

# [hack:iphoneX底部适配](https://www.jianshu.com/p/578db27f0adf)

iphoneX的底部适配

在项目中因为底部tab栏在iphonex的显示问题

1. 方法一

```css
@media only screen and (width: 375px) and (min-height: 690px){
  #app {
     padding-bottom: 0.34rem;
   }
 }
```

1. 方法二

第一步：新增 viweport-fit 属性，使得页面内容完全覆盖整个窗口：

```xml
<meta name="viewport" content="width=device-width, viewport-fit=cover">
```

第二步：页面主体内容限定在安全区域内

```css
body {
  padding-bottom: constant(safe-area-inset-bottom);
  padding-bottom: env(safe-area-inset-bottom);
}
```

第三步：fixed 元素的适配

- 类型一：fixed 完全吸底元素（bottom = 0）

```css
{
  padding-bottom: constant(safe-area-inset-bottom);
  padding-bottom: env(safe-area-inset-bottom);
}
```

- 类型二：fixed 非完全吸底元素（bottom ≠ 0），比如 “返回顶部”、“侧边广告” 等

```css
{
  margin-bottom: constant(safe-area-inset-bottom);
  margin-bottom: env(safe-area-inset-bottom);
}
```

作者：ismyshellyiqi
链接：https://www.jianshu.com/p/578db27f0adf
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

# [iPhone X 适配 手Q H5页面通用解决方案](https://zhuanlan.zhihu.com/p/30840440)

# [如何写一个适配iPhoneX的底部导航](https://mp.weixin.qq.com/s/6mu1PMkURvPvxpGuZM6uFQ)

# [手机端各种奇葩问题总录](https://github.com/zhongDZ/zhongdz.github.com/issues/37#)#37