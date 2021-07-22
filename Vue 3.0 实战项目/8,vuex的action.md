**Mutations必须是同步请求**

actions可以实现异步操作。

 actions接受一个与store具有相同方法和属性的context对象，

所以在actions中可以用context来提交mutations 

![image-20210304155544476](media\image-20210304155544476.png) 

在组件中调用actions方法，可以用dispatch方法调用。

**注意：**在mutation中添加异步请求，容易破坏vuex的可追溯性。



### async和awiat

用async修饰函数，返回值会被包裹成**promise对象**

![image-20210506144612790](media/image-20210506144612790.png) 

![image-20210506144644102](media/image-20210506144644102.png) 

 await只能在async函数中使用，使得只有在await后面的语句有结果后才能继续执行下面的语句。

**await后面必须跟一个promise对象。**

### 优化重复代码

![image-20210506151444016](media/image-20210506151444016.png) 

### 小技巧

在vscode中鼠标移到变量上可以看到变量的ts定义，然后再从对应的包中引入类型定义。

![image-20210506150308096](media/image-20210506150308096.png) 

![image-20210506150326106](media/image-20210506150326106.png) 

### 延时函数

![image-20210506152032719](media/image-20210506152032719.png) 

##  teleport

