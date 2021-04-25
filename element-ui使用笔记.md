```vue
<template v-for="(person,index) in Form.List">
    <el-form-item :prop="'List.'+index + '.code'" :key="person.key" :rules="rules">
        <el-input v-model="person.code"></el-input>
    </el-form-item>
    <el-form-item prop="name">
        <el-input v-model="person.name"></el-input>
    </el-form-item>
    <el-button @click="removeDomain(index)" size="small">-</el-button>
</template>
```

# el-form-item 设置 prop 报错：please transfer a valid prop path to form item!

el-form-item 里面的循环prop名字，需要 和form中list的名字一致，这样才能确保组件的统一性。

```vue
<template scope="scope">
          <el-form-item  label="姓名" :prop="'data.' + scope.$index  + '.a'" :rules="{ required: true, message: '请输入姓名', trigger: 'blur' }">
           <el-input type="text" v-model="scope.row.a"></el-input>
         </el-form-item>
        </template>
```

