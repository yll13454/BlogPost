一、store文件夹目录介绍如下图所示，store文件夹下一级目录有index.ts 初始化vuex的配置, interface.ts 设置root层state类型, modules用来存放vuex子模块定义；

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70)

### 定义state顶层数据结构

定义 root 顶层 state 的数据类型， 在store/interface.ts 中，比如我们在state上定义属性 test 的类型, 然后默认导出该类型定义

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-16293557254364)

在 store/index.ts 中，配置顶层store，在调用 createStore 的时候根据将上一步设置的类型定义在此处传递进去 createStore<RootStateTypes>({ ... })

### 唯一的key

该key在注册store时需要用到, export const key: InjectionKey<Store<RootStateTypes>> = Symbol('vue-store');。

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-16293557956996)

该key值的使用，在使用store的时候需要引入，如下图所示，该处use的时候一定要入参第二个参数key

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-16293558146118)

**在页面中使用，推荐使用 Composition API，在vue组件setup中使用**

**import { useStore } from 'vuex';**

**const store = useStore(key); // 便可实现调用**

当然有时候会觉得很麻烦, 每次都要引入key，可以在 store/index.ts中统一导出，如下图所示，使用就可以直接调用了。

其中 baseUseStore 为 import { useStore as baseUseStore } from 'vuex'

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-162935589581110)

import { useStore } from '@/store';

const store = useStore(); // 便可实现调用

### 定义 modules 中子模块，以 test 举例

\1. 定义 state 数据类型，在store/modules/test/interface.ts中定义，如下所示，有两个字段 name, count

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-162935610936212)

2.定义子模块，

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-162935621559514)

### 3.子模块引入使用，

首先在 store/interface.ts中增加一个全量的state集成，AllStateTypes，extends至根节点 的 RootStateTypes，将test子模块的类型定义引入，并在该处定义便可，这里的键 testModule 为接下来将在store/index.ts中引入的键定义，如下图所示，新增类型定义，后续如果新增一个模块均在此全量的 AllStateTypes 新增便可

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-162935764339716)

\4. 在 store/index.ts中的处理，在modules中引入 testModule子模块，在 baseUseStore 中我们需要手动传入泛型，baseUseStore<AllStateTypes>(key)，这样，项目中就能使用所有的状态数据不会提示报错也不要在每个地方定义了，当然，如下图所示，我们保留泛型的传入，将默认的类型定义为 AllStateTypes 也是可以的

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-162935773095718)

在使用的时候，从 @/store中引入useStore, 可以直接调用子模块的各个数据，当然也可以使用commit, dispatch等方法进行处理，如下图demo所示

![img](media/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbndlaWxpbjAxMjM=,size_16,color_FFFFFF,t_70-162935773362920)

总结：建议state类型定义单独放在一个文件中处理，方便管理，各个子模块的类型定义单独放在子模块的文件夹中处理，最后统一在根节点上 集成便可，当然，如果有很多公用的类型可以抽取出来。

