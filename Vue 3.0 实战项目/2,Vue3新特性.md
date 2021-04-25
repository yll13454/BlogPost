![image-20210207132150672](D:\笔记\Vue 3.0 实战项目\media\image-20210207132150672.png) 

![image-20210207132215524](D:\笔记\Vue 3.0 实战项目\media\image-20210207132215524.png) 

 ![image-20210207132925658](D:\笔记\Vue 3.0 实战项目\media\image-20210207132925658.png) 

 

### setup(){}

props,data,computed,methods生命周期之前运行的。

在setup中访问不到this关键字。

### 响应式

![image-20210207154940483](D:\笔记\Vue 3.0 实战项目\media\image-20210207154940483.png) 

![image-20210208131534565](D:\笔记\Vue 3.0 实战项目\media\image-20210208131534565.png) 

### 生命周期

![image-20210207161816988](D:\笔记\Vue 3.0 实战项目\media\image-20210207161816988.png) 

![image-20210207161855693](D:\笔记\Vue 3.0 实战项目\media\image-20210207161855693.png) 

![image-20210207161929115](D:\笔记\Vue 3.0 实战项目\media\image-20210207161929115.png) 

最后俩个是调试用的钩子函数

参数是event,里面包含

![image-20210207162508910](D:\笔记\Vue 3.0 实战项目\media\image-20210207162508910.png)

![image-20210207162632823](D:\笔记\Vue 3.0 实战项目\media\image-20210207162632823.png) 

### reactive（）

类型是一个函数，参数是object类型。

**注意：**在reactive里面操作数据不许对数据的value操作，直接操作数据本身就好。

** 在reactive数据集中如果使用computed，会导致它的ts数据类型推断出错，显示为any类型，可以不管，也可以声明接口来指定数据的类型解决问题。

![image-20210207163346431](D:\笔记\Vue 3.0 实战项目\media\image-20210207163346431.png) 

1，声明数据类型

![image-20210207163523852](D:\笔记\Vue 3.0 实战项目\media\image-20210207163523852.png) 

2，

![image-20210207163603164](D:\笔记\Vue 3.0 实战项目\media\image-20210207163603164.png) 

**响应式数据类型为ref类型**

### toRefs()

对于一个响应式的对象，当从中取值的时候，他的对应属性就是一个普通的数据类型，而不是一个响应式的类型。

这个时候就需要toRefs()，接受一个reactive对象为参数，返回一个普通对象，但是得到的这个对象，他的每一项都变成了ref类型的对象，也就是响应式数据类型。

![image-20210208131453860](D:\笔记\Vue 3.0 实战项目\media\image-20210208131453860.png) 

### ref和reactive的区别

### watch

![image-20210208145621785](D:\笔记\Vue 3.0 实战项目\media\image-20210208145621785.png) 

它的newvalue和oldvalue也是数组类型。

 ![image-20210208150225158](D:\笔记\Vue 3.0 实战项目\media\image-20210208150225158.png) 

watch的值不能是基本数据类型的值，必须是函数，ref，reactive Object，array

## 泛型

对于未知的类型，可以用泛型来预先束缚结果的类型，在用 的时候在使用与环境相匹配的类型。

![image-20210208200700088](D:\笔记\Vue 3.0 实战项目\media\image-20210208200700088.png)

![image-20210208200721772](D:\笔记\Vue 3.0 实战项目\media\image-20210208200721772.png)

### definecomponent

![image-20210208205505130](D:\笔记\Vue 3.0 实战项目\media\image-20210208205505130.png) 

在defineComponent中的setup有两个参数，props可以访问到组件传过来的值，

props是响应式对象

在setup中无法访问到this对象，context提供了最常用的三个属性。分别对应其中的属性，插槽，事件。

### teleport瞬间转移 标签

 teleport组件将在标签内的组件传送到指定的dom标签上

在父组件或者更组件上定义

![image-20210209134236852](D:\笔记\Vue 3.0 实战项目\media\image-20210209134236852.png) 

![image-20210209134312559](D:\笔记\Vue 3.0 实战项目\media\image-20210209134312559.png) 

### 组件属性emits

这个属性来指代向外发射的自定义事件

![image-20210209135712154](D:\笔记\Vue 3.0 实战项目\media\image-20210209135712154.png) 

也可以不添加参数验证

![image-20210209135753603](D:\笔记\Vue 3.0 实战项目\media\image-20210209135753603.png) 

###  Suspense组件

如果使用suspense组件在setup中要返回一个promise对象 ，而不是对象。 

这个组件包裹的组件，得等其他组件完成渲染之后才会渲染他本身。

![image-20210209155026829](D:\笔记\Vue 3.0 实战项目\media\image-20210209155026829.png) 

它会等default下面的俩个组件都展示之后才会显示。

优先执行fallback这个插槽的dom，只有当default的结果返回的时候才会关闭fallback的这一项，展现default这边的数据。

#### 抓取Suspense组件内错误onErrorCaptured

 参数是function，e为function的参数

它返回一个boolean值，表示这个错误是否向上传播。

![image-20210209164933368](D:\笔记\Vue 3.0 实战项目\media\image-20210209164933368.png) 

![image-20210209165352667](D:\笔记\Vue 3.0 实战项目\media\image-20210209165352667.png) 

![image-20210209165428107](D:\笔记\Vue 3.0 实战项目\media\image-20210209165428107.png) 

![image-20210209165441586](D:\笔记\Vue 3.0 实战项目\media\image-20210209165441586.png) 

![image-20210209165544834](D:\笔记\Vue 3.0 实战项目\media\image-20210209165544834.png) 

![image-20210209165610409](D:\笔记\Vue 3.0 实战项目\media\image-20210209165610409.png) 

![image-20210209165637266](D:\笔记\Vue 3.0 实战项目\media\image-20210209165637266.png) 