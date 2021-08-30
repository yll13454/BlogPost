## SSO单点登录

### 概念

SSO 英文全称 Single Sign On，单点登录。

在多个应用系统中，只需要登录一次，就可以访问其他相互信任的应用系统。

#### 流程图

![图片](media\640) 

系统A和系统B：使用token认证登录。

SSO 认证中心：使用会话认证登录。

前后端分离项目，登录使用token进行解决，前端每次请求接口时都必须传递token参数。

#### 退出

![图片](media\640) 

#### Token 生成方式

创建全局会话可以使用session，将session存储到redis中。

令牌的生成可以使用JWT。

PHP JWT参考地址：https://github.com/lcobucci/jwt

当然还可以自定义token的生成方式。

#### SSO与OAuth的区别

SSO是处理一个公司内的不同应用系统之间的登录问题，比如阿里巴巴旗下有很多应用系统，我们只需要登录一个系统就可以实现不同系统之间的跳转。

OAuth是不同公司遵循的一种授权方案，也是一种授权协议，通常都是由大公司提供，比如腾讯，微博。我们常用的QQ登录，微博登录等，使用OAuth的好处是可以使用其他第三方账号进行登录系统，减少了因用户懒，不愿注册而导致用户流失的风险。

#### SSO与RBAC的关系

如果企业有多个管理系统，现由原来的每个系统都有一个登录，调整为统一登录认证。

那么每个管理系统都有权限控制，吸取统一登录认证的经验，我们也可以做一套统一的RBAC权限认证。

### CAS

SSO 仅仅是一种架构，一种设计，而 CAS 则是实现 SSO 的一种手段。两者是抽象与具体的关系。当然，除了 CAS 之外，实现 SSO 还有其他手段，比如简单的 cookie。

CAS （Central Authentication Service）中心授权服务，本身是一个开源协议，分为 1.0 版本和 2.0 版本。1.0 称为基础模式，2.0称为代理模式，适用于存在非 Web 应用之间的单点登录。

### 1.同域 SSO

![img](media\d5c406051e343391aa970e8960229356) 

用户访问产品 a，向 后台服务器发送登录请求。

登录认证成功，服务器把用户的登录信息写入 session。

服务器为该用户生成一个 cookie，并加入到 response header 中，随着请求返回而写入浏览器。该 cookie 的域设定为 dxy.cn。

下一次，当用户访问同域名的产品 b 时，由于 a 和 b 在同一域名下，也是 dxy.cn，浏览器会自动带上之前的 cookie。此时后台服务器就可以通过该 cookie 来验证登录状态了。

### 2.同父域 SSO

同父域 SSO 是同域 SSO 的简单升级，唯一的不同在于，服务器在返回 cookie 的时候，要把cookie 的 domain 设置为其父域。

比如两个产品的地址分别为 a.dxy.cn 和 b.dxy.cn，那么 cookie 的域设置为 dxy.cn 即可。在访问 a 和 b 时，这个 cookie 都能发送到服务器，本质上和同域 SSO 没有区别。

### 3.跨域 SSO

但是当两个产品不同域时，cookie 无法共享，所以我们必须设置独立的 SSO 服务器了。这个时候，我们就是通过标准的 CAS 方案来实现 SSO 的。

#### 详解CAS

CAS 1.0 协议定义了一组术语，一组票据，一组接口。

### 术语：

- Client：用户。
- Server：中心服务器，也是 SSO 中负责单点登录的服务器。
- Service：需要使用单点登录的各个服务，相当于上文中的产品 a/b。

### 接口：

- /login：登录接口，用于登录到中心服务器。
- /logout：登出接口，用于从中心服务器登出。
- /validate：用于验证用户是否登录中心服务器。
- /serviceValidate：用于让各个 service 验证用户是否登录中心服务器。

### 票据

- TGT：Ticket Grangting Ticket 

TGT 是 CAS 为用户签发的登录票据，拥有了 TGT，用户就可以证明自己在 CAS 成功登录过。TGT 封装了 Cookie 值以及此 Cookie 值对应的用户信息。当 HTTP 请求到来时，CAS 以此 Cookie 值（TGC）为 key 查询缓存中有无 TGT ，如果有的话，则相信用户已登录过。

- TGC：Ticket Granting Cookie

CAS Server 生成TGT放入自己的 Session 中，而 TGC 就是这个 Session 的唯一标识（SessionId），以 Cookie 形式放到浏览器端，是 CAS Server 用来明确用户身份的凭证。

- ST：Service Ticket 

ST 是 CAS 为用户签发的访问某一 service 的票据。用户访问 service 时，service 发现用户没有 ST，则要求用户去 CAS 获取 ST。用户向 CAS 发出获取 ST 的请求，CAS 发现用户有 TGT，则签发一个 ST，返回给用户。用户拿着 ST 去访问 service，service 拿 ST 去 CAS 验证，验证通过后，允许用户访问资源。



[前端需要了解的 SSO 与 CAS 知识](https://juejin.cn/post/6844903509272297480)

