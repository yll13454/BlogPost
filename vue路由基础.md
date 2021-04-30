### **vue路由中的传参**

**query：**

```html
<router-link :to=``"{ name: 'W', query: { id:'1234'，age:'12' }}"``/>
<router-link :to=``"{ path: '/W', query: { id:'1234',age:'12' }}"``/>
```

name和path都可以用

前者的routes基于name设置

```json
{
``path: ``'/hhhhhhh'``, ``//这里可以任意
``name: ``'W'``, ``//这里必须是W
``component: W``
}
```

然后就把path匹配添加到url上去

```
http:``//localhost:8080/#/hhhhhhh?id=1234&age=12
```

后者基于path来设置routes

```json
{
``path: ``'/W'``, ``//这里必须是W
``name: ``'hhhhhhhh'``, ``//这里任意
``component: W``
}
```

url:http://localhost:8080/#/W?id=1234&age=12

这两种方法，都可以自定义path的样式，
不过一个是在router-link to里面定义，一个则是在routes里面定义
在接收参数的时候都是使用this.$route.query.id

**parmas：**

```html
<router-link :to=``"{ name: 'W', params: { id:'1234',age:'12' }}"``/>
```

**这里只能用name不能用path，不然会直接无视掉params中的内容**
然后在routes中添加

```json
{
`path:``'/W/:id/:age'``,
``name:``'W'``,
``component:W``
}
```

这里的name与上面router-link中的name保持一致

url就取决于这个path的写法http://localhost:8080/#/W/1234/12

注意，path里面的/w可以任意写，写成/hhhhh也可以

但是！

/:id和/:age不能省略，且不能改名字

不写的话，第一次点击可以实现组件跳转

且可以通过this.$route.parmas.id获取到传过来的id值，但如果

刷新页面，传过来的id值和age值就会丢失





1.$router为VueRouter实例，想要导航到不同URL，则使用$router.push方法

2.$route为当前router跳转对象，里面可以获取name、path、query、params等