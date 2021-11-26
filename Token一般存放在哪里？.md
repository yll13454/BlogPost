### Token是什么？

Token 其实就是访问资源对凭证。

一般是用户通过用

### Token 放在 cookie、localStorage、sessionStorage中的不同点？

#### 1 将Token存储于localStorage或 sessionStorage

Web存储（localStorage/sessionStorage）可以通过同一域商Javascript访问。这意味着任何在你的网站上的运行的JavaScript都可以访问Web存储，所以容易受到XSS攻击。尤其是项目中用到了很多第三方JavaScript类库。

建议不要在Web存储中存储任何有价值或信任任何Web存储中的信息。 这包括会话标识符和令牌。作为一种存储机制，Web存储在传输过程中不强制执行任何安全标准。

>XSS攻击：Cross-Site Scripting（跨站脚本攻击）简称XSS，是一种代码注入攻击。攻击者通过在目标网站注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可以获取用户的敏感信息如Cookie、SessionID等，进而危害数据安全。

#### 2 将Token存储与cookie

**优点：**

- 可以制定httponly，来防止被JavaScript读取，也可以制定secure，来保证token只在HTTPS下传输。

**缺点：**

- 不符合Restful 最佳实践。 [Restful最佳实践](https://juejin.cn/post/6844903941403049998)
- 容易遭受CSRF攻击（可以在服务器端检查Refer和Origin）

>CSRF:跨站请求伪造，简单的说，是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并运行一些操作（如：发邮件、发信息、甚至财产操作如转账和购买商品）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去运行。这利用了web中用户身份验证的一个漏洞：简单的身份验证职能保证请求发自某个用户的浏览器，却不能保证请求本身是用户自愿发出去的。CSRF并不能够拿到用户的任何信息，它只是欺骗用户浏览器，让其以用户的名义进行操作。

### 总结

关于token 存在cookie还是localStorage有两个观点。

- 支持Cookie的开发人员会强烈建议不要将敏感信息（例如JWT)存储在localStorage中，因为它对于XSS毫无抵抗力。
- 支持localStorage的一派则认为：撇开localStorage的各种优点不谈，如果做好适当的XSS防护，收益是远大于风险的。

放在cookie中看似看全，看似“解决”（因为仍然存在XSS的问题）一个问题，却引入了另一个问题（CSRF）

localStorage具有更灵活，更大空间，天然免疫 CSRF的特征。Cookie空间有限，而JWT一半都占用较多字节，而且有时你不止需要存储一个JWT。

不懂什么是JWT的参考阅读[【什么是JWT】](https://juejin.cn/post/6909767910818840583)

确保你的代码以及第三方库的代码有足够的XSS检查，在此之上将token存放在localStorage中。

> 在XSS面前，即便你的httpOnly cookie无法被获取，黑客依然可以诱导或者在用户毫不知情的情况下做任何事情。记住！黑客的代码和你的代码一样被用户信任！XSS只要存在那么无论将信息存储在cookie还是localStorage，都是一样脆弱不堪，唯一的区别只是获取难度。XSS漏洞很难被发现，因为一个网站的构建不仅仅是基于你自己的代码，第三方的代码同样已可能存在XSS。


作者：Hecate
链接：https://juejin.cn/post/6922782392390746125
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。