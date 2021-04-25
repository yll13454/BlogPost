### session

http没有状态

服务器的session，用来存储用户信息

服务器用session保存用户的状态，客户端用cookie保存session，每次访问都呆着session，用来设别用户。

每次请求都会经过中间件，所以中间件会执行多次