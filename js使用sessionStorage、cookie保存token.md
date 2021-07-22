**登录成功的token：**

![img](media/20180604201847857)

**其他请求的时候，在header里面带上token：**

**![img](media/20180604201957619)**

### js使用cookie保存token(cookie在http请求中，随着请求发送到服务器)

将token保存在cookie中，一旦浏览器关闭，cookie中的token就会被清空。

 

document.cookie = token;                          //将token保存在cookie中

var token = document.cookie.split(";")[0];    //从cookie中读取token

### js使用sessionStorage保存token


sessionStorage.setItem("key","value");    

 

//保存数据到sessionStorage

var data = sessionStorage.getItem（"key"）;   //获取数据

sessionStorage.removeItem("key");                //删除数据

sessionStorage.clear();                                  //删除保存的所有数据