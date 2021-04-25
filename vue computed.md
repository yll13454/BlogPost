

Vue的计算属性：

1.在computed属性对象中定义计算属性的方法，在页面中使用{{方法名}}来显示计算的结果。

2.通过getter\setter实现对属性数据的显示和监视，计算属性存在缓存，多次读取只执行一次getter计算。

```vue

<div id="demo">
		姓：<input type="text" placeholder="firstName" v-model="firstName"><br>
		名：<input type="text" placeholder="lastName" v-model="lastName"><br>
		姓名1(单向):<input type="text" placeholder="FullName1" v-model="fullName1"><br>
		姓名2(双向):<input type="text" placeholder="FullName2" v-model="fullName2"><br>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    const demo = new Vue({
		el : '#demo',
		data : {
			firstName : 'A',
			lastName : 'B',
			fullName2 : 'A B'
		},
		computed : {//计算属性相当于data里的属性
			//什么时候执行：初始化显示/ 相关的data属性发生变化
			fullName1(){//计算属性中的get方法，方法的返回值就是属性值
				return this.firstName + ' ' + this.lastName
			},
 
			fullName3 : {
				get(){//回调函数 当需要读取当前属性值是执行，根据相关数据计算并返回当前属性的值
					return this.firstName + ' ' + this.lastName
				},
				set(val){//监视当前属性值的变化，当属性值发生变化时执行，更新相关的属性数据
					//val就是fullName3的最新属性值
					console.log(val)
					const names = val.split(' ');
					console.log(names)
					this.firstName = names[0];
					this.lastName = names[1];
				}
			}
		}
 
	})
 
 
</script>
 
```

默认情况下computed的set方法是不会被执行，只有对重新赋值才会触发computed的set方法。

### 总结： 

1.如果一个数据依赖于其他数据，那么把这个数据设计为computed的  

2.如果你需要在某个数据变化时做一些事情，使用watch来观察这个数据变化