### history API

 history.pushState()跳转页面的时候不会刷新页面

三个参数：

- 状态参数，导航结束后会触发popstate事件，该事件的state属性包含该历史记录状态对象的副本。
- 标题， 被浏览器忽略。
- URL 新的URL，可选，可缺省。**必须与当前URL处于同一个域，即新URL必须与当前URL同源，否则pushState()会抛出异常**

### router使用

history参数

- hash模式，通过改变URL字符串中#后面的字符来切换页面，且不刷新页面，适用于旧版浏览器。
- history模式

history中可选三个函数参数，createMemoryHistory()，createWebHashHistory()，createWebHistory()

![image-20210226131532461](media\image-20210226131532461.png) 

# URL结构

![image-20210226132547085](media\image-20210226132547085.png) 

### useRouter

返回 [router](https://next.router.vuejs.org/zh/api/#router-properties) 实例。相当于在模板中使用 `$router`。必须在 `setup()` 中调用。

### router-link的跳转方式

方法一

![image-20210226145756660](media\image-20210226145756660.png) 

方法二

![image-20210226145830739](media\image-20210226145830739.png) 

useRouter//定义路由的行为

useRoute//获取路由的信息

例子

```{
setup(){
	const router = userRouter()
    router.push({name:'path',params:{id:1}})
}
```

![image-20210226153334275](media\image-20210226153334275.png) 

![image-20210226160549331](media\image-20210226160549331.png)

## vuex状态管理工具

![image-20210226161716852](media\image-20210226161716852.png)

#### 流程

![image-20210226162355643](media\image-20210226162355643.png)

vuex解决俩大问题，

1）数据层层传递的

2）数据同步

核心是一个仓库store

####  安装

```
npm install vue@next --save
```

#### 使用

```
import {createStore} from 'vuex'

const store = createStore({
	state：{
	...数据
	},
	mutations:{
		add(){
		
		},
		...操作数据的方法
	}
})

store.commit('add')//调用mutations中方法
```

createStore接收对象作为参数

![image-20210302132439056](media\image-20210302132439056.png) 

createStore接收一个泛型，返回一个包裹这个类型的state ，

可以自定义泛型来确定返回的类型。

####  在组件中使用vuex

```
import {useStore} from 'vuex'

export default defineComponent({
	setup(){
		const store = userStore<导入的store文件中声明的泛型>()
		
	}
})
```

**为了获得更全面的自动补全，可以在使用store的时候引入对应的泛型来获得自动补全。**

### getter

类似于组件的computed，缓存，根据依赖值缓存起来。依赖变化，值才变化。

使用

```js
getters:{
	biggerColumnsLen(state){
		retrun state.columns.filter(c=>c.id>2).length
	}
}
...

//其他组件中
store.getters.biggerColumnsLen
```

#####   用法提升 

返回一个函数，用来实现给getter传参。

例子：

```
getters:{
	getColumnsById:(state)=>(id:number)=>{
		return state.columns.find(c=>c.id===id)
	},
	getPostsById:(state)=>(cid:nubmer)=>{
		return state.posts.filter(c=>c.columnsId===cid)
	}
}
...


//组件中
setup(){
	const column = computed(()=>{store.getters.getColumnsById(currentId)})
	const list = computed(()=>{store.getters.getPostsById(currentId)})
}

```

mutations参数

```
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

```
store.commit('increment', 10)
```

### router

##### 导航守卫

router.beforeEach((to,form,next)=>{})

可以根据元信息来判断路由是否跳转

![image-20210303132839637](media\image-20210303132839637.png) 

![image-20210303132904370](media\image-20210303132904370.png) 