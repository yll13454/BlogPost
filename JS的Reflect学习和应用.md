**反射机制**:

> 反射机制指的是程序在运行时能够获取自身的信息

## 1 JS的反射对象

> Reflect是一个内建的对象，用来提供方法去拦截JavaScript的操作。Reflect不是一个函数对象，所以它是不可构造的，也就是说它不是一个构造器，你不能通过`new`操作符去新建或者将其作为一个函数去调用Reflect对象。Reflect的所有属性和方法都是静态的。

### 1.1 为什么需要Reflect对象

for..in可以实现,`Array.isArray`/`Object.getOwnPropertyDescriptor`，或者甚至`Object.keys`都是可以归类到反射这一类中。