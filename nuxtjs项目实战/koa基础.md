# 利用koa-generator快速搭建一个koa2项目

\1.  npm install koa-generator -g 安装koa-generator，利用koa-generator快速搭建Node.js服务器

\2. koa2 my-project 新建一个叫做my-project的koa2项目

\3. cd my-project   npm install

\4. 启动项目 npm start

\5. 打开 localhost:3000



 koa2 -e my-project  //-e是指用ejs模版引擎



### 模版引擎 ejs

![image-20210104141005229](D:\笔记\nuxtjs项目实战\media\image-20210104141005229.png)

### async和await

async是指异步函数。await使用必须在async函数内。

await后面是一个promise对象，如果后面不是promise对象则await会把结果包装成promise对象。

### 中间件

中间件引入私有 

![image-20210104144902652](D:\笔记\nuxtjs项目实战\media\image-20210104144902652.png)

![image-20210104150437956](D:\笔记\nuxtjs项目实战\media\image-20210104150437956.png)

await next(); //这个中间件执行完毕，交给下一个中间件处理。

### koa 路由

![image-20210104153210083](D:\笔记\nuxtjs项目实战\media\image-20210104153210083.png)

### 注意：

异步数据

asyncData处理组件数据

fetch()处理跟vuex的数据

![image-20210105165752931](D:\笔记\nuxtjs项目实战\media\image-20210105165752931.png)

### vuex

![image-20210105165958621](D:\笔记\nuxtjs项目实战\media\image-20210105165958621.png)