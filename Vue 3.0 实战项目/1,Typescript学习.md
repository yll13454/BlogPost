 

![image-20210203165747247](D:\笔记\Vue 3.0 实战项目\media\image-20210203165747247.png)

动态：运行期间进行数据类型检测

静态：编译阶段进行数据类型检测

 

元组：多类型数组。初始定义时，添加对应位置的数据类型，数组添加时，只能选已定义的数据类型。

#### 命令行

```
tsc -v//查看typescript本地版本
```

undefined是所有类型的子类型，可以赋值给所有类型

#### JavaScript的数据类型

7种基本数据类型和object

基本数据类型：number，null，undefined，string，BigInt，boolean，symbol

- 数组：数组和类数组，类数组有部分数组方法。

- 元组：多类型数组。初始定义时，添加对应位置的数据类型，数组添加时，只能选已定义的数据类型。

![image-20210203201248061](.\media\image-20210203201248061.png)

- interface 接口 

  1，对对象的形状（shape）进行描述

  2，鸭子类型 

![image-20210203202914344](D:\笔记\Vue 3.0 实战项目\media\image-20210203202914344.png) 

- Function

**注意：**在ts文件中，只要在：后面都是在声明类型

![image-20210203203255840](D:\笔记\Vue 3.0 实战项目\media\image-20210203203255840.png)

声明接口，他是函数类型

![image-20210203203428232](D:\笔记\Vue 3.0 实战项目\media\image-20210203203428232.png)

- 类型推论

- 联合类型

  ```
  let x :string | number
  ```

  **注意：**联合类型只能访问几种类型共有的属性和方法。

- 类型断言 as(union types)

告诉编译器，你比它更了解这个变量类型。

![image-20210203205540427](D:\笔记\Vue 3.0 实战项目\media\image-20210203205540427.png)

- type guard(假如)

typeof

instanceof

![image-20210204133520695](D:\笔记\Vue 3.0 实战项目\media\image-20210204133520695.png)

#### 类class

![image-20210204191334343](D:\笔记\Vue 3.0 实战项目\media\image-20210204191334343.png)

- 封装

![image-20210204192514527](D:\笔记\Vue 3.0 实战项目\media\image-20210204192514527.png)

- 继承

![image-20210204192551402](media\image-20210204192551402.png)

- 多态

![image-20210204192619402](media\image-20210204192619402.png)

**静态属性：**static 属性名   **或**  static 方法名

静态属性和方法不需要实例化，直接在类上面调用就行。

![image-20210204192946742](media\image-20210204192946742.png)

 protected修饰的属性和方法，只能被本身和子类使用。



#### 类和接口

![image-20210204201854291](media\image-20210204201854291.png)

 

![image-20210204202045885](media\image-20210204202045885.png)

或

![image-20210204202123348](media\image-20210204202123348.png)

#### 枚举enums

常量，取一定范围内的一系列常量，就是枚举。 

![image-20210205141839587](D:\笔记\Vue 3.0 实战项目\media\image-20210205141839587.png)

枚举的其中一个被赋值，其余的会依次递增。

上面代码编译后

![image-20210205142028413](D:\笔记\Vue 3.0 实战项目\media\image-20210205142028413.png)

会将up赋值为0，之后Direction[0]="up",

**常量枚举**=>可提升性能。加上const就是常量枚举，编译之后没有多余的代码，

![image-20210205142428659](D:\笔记\Vue 3.0 实战项目\media\image-20210205142428659.png)

编译后

![image-20210205142515322](D:\笔记\Vue 3.0 实战项目\media\image-20210205142515322.png)

**注意：**枚举的值有俩种常量值和计算值。只有常量值才有常量枚举。

### 泛型

想要解决的问题

定义函数，接口，类的时候，我们不予先指定类型，而是在使用的时候在指定类型和特征。 

 相当于一个占位符，或者变量。

![image-20210205144652877](D:\笔记\Vue 3.0 实战项目\media\image-20210205144652877.png)

![ ](D:\笔记\Vue 3.0 实战项目\media\image-20210205145127849.png)

- 约束泛型

约束变量的条件。

![image-20210205150848733](D:\笔记\Vue 3.0 实战项目\media\image-20210205150848733.png)

要求每个声明的变量中都有length这个属性。 

#### 泛型在类上面的应用

![image-20210205152529225](D:\笔记\Vue 3.0 实战项目\media\image-20210205152529225.png)

限定类型统一。

![image-20210205160622835](D:\笔记\Vue 3.0 实战项目\media\image-20210205160622835.png)

### type aliase(类型别名)

![image-20210205161347439](D:\笔记\Vue 3.0 实战项目\media\image-20210205161347439.png)

### 字面量

只能是原始数据类型

![image-20210205161630054](D:\笔记\Vue 3.0 实战项目\media\image-20210205161630054.png)

交叉类型

![image-20210205161756773](D:\笔记\Vue 3.0 实战项目\media\image-20210205161756773.png)

### 声明文件

使用关键字declare告诉ts这个变量已经在别处定义了，你拿来用就好了

通常会将声明语句放到单独的文件中去。以xxx.d.ts结尾

它并没有真的定义这个变量，只是定义了这个类型。会用于编译时的检查。并不是实现功能的真正代码。在其他地方可以获得它的类型定义。

![image-20210205163126642](D:\笔记\Vue 3.0 实战项目\media\image-20210205163126642.png)

### 内置对象

