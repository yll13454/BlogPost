网上流传着多种匹配URL的正则表达式版本，但我经过试验，最好用的还是从stackoverflow上查到的：

(https?|ftp|file)://[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]

IP地址、前后有汉字、带参数的，都是OK的。

![image](https://images2015.cnblogs.com/blog/3787/201601/3787-20160104090529465-1297988082.png)

[正确匹配URL的正则表达式](https://www.cnblogs.com/speeding/p/5097790.html)

