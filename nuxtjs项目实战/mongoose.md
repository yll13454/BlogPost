### mongoose操作顺序

在dbs文件夹的models中创建表模型

- 文件名就是数据库中的表名

表中的字段，通过schema创建

![image-20210104175311947](D:\笔记\nuxtjs项目实战\media\image-20210104175311947.png)



### 实例应用步骤

#### 写入数据库

在路由中引入表模型

![image-20210104170459462](D:\笔记\nuxtjs项目实战\media\image-20210104170459462.png)

接口中新建实例 

post方法才能拿到request的数据

get方法能拿刀query的数据

 

![image-20210104173343446](D:\笔记\nuxtjs项目实战\media\image-20210104173343446.png)

save()方法是module中的方法，

Schema描述了表中的字段，具备了数据库中的一些操作，例如增删改查，这些是在module中定义的，所以实例可以用这些方法。

在数据库操作中用try-catch来捕获错误

![image-20210104174017396](D:\笔记\nuxtjs项目实战\media\image-20210104174017396.png)



![image-20210104174852079](D:\笔记\nuxtjs项目实战\media\image-20210104174852079.png)

curl命令行 

-d 表示方法是post

#### 读取数据库

理解为一个类的静态方法和实例方法。module

![image-20210104190718393](D:\笔记\nuxtjs项目实战\media\image-20210104190718393.png)

![image-20210104192743406](D:\笔记\nuxtjs项目实战\media\image-20210104192743406.png)

![image-20210104193101505](D:\笔记\nuxtjs项目实战\media\image-20210104193101505.png)

### 注意

- 每个接口都得ctx.body赋值
- 请求都是异步的都必须用await修饰变成同步