**Mutations必须是同步请求**

actions可以实现异步操作。

 actions接受一个与store具有相同方法和属性的context对象，

所以在actions中可以用context来提交mutations 

![image-20210304155544476](D:\笔记\Vue 3.0 实战项目\media\image-20210304155544476.png) 

在组件中调用actions方法，可以用dispatch方法调用。

**注意：**在mutation中添加异步请求，容易破坏vuex的可追溯性。



