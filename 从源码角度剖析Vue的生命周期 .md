 [[Vue.js进阶\]从源码角度剖析Vue的生命周期](https://juejin.cn/post/6844903821462749191#heading-5)

每个 .vue 文件可以理解为一个构造函数，或者一个 Class，而在父组件中引用组件就等于对其的实例化

```vue
  <view>
    <custom-textarea v-model="text1"></custom-textarea>
    <custom-textarea v-model="text2"></custom-textarea>
  </view>
复制代码
```

上述代码即创建了 2 个 CustomTextarea 组件的实例

熟悉面向对象的同学应该知道，构造函数实例化时，同时会创建实例的属性和方法，一般每个实例的属性都不相同，而方法因为是函数，所以会复用，已达到节省内存的效果

```javascript
class Person {
  constructor(name) {
    this.name = name
  }
  eat() {}
}

const person1 = new Person('张三')
const person2 = new Person('李四')

console.log(person1.name === person2.name) // false
console.log(person1.eat === person2.eat) // true
复制代码
```

Vue2  的组件借鉴了面向对象的原理，虽然内部的实现方式不同，但最终的行为一致，即**组件的每个实例都拥有不同的 data，但会复用相同的 methods**

和 methods 对象相同，computed 对象的属性名是一个响应式变量，而值是一个函数，所以所有实例也会指向同一个函数，但由于这个函数需要有返回值，所以不会用防抖函数进行包裹，很少遇到函数公用导致的问题

而 watch 也和 methods 对象相同，所有组件实例共用，所以也会存在防抖的问题

至于生命周期本身就是一个函数，如果对生命周期设置了防抖，多个组件实例同时初始化时也会造成只执行一次的情况


作者：yeyan1996
链接：https://juejin.cn/post/6892577964458770445
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

![img](media/16a0f60d4c25fc9dtplv-t2oaga2asx-watermark.awebp)

