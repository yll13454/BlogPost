# 一、使用场景

常用于form表单中输入框组件的校验

# 二、使用方法

## 1. 表单校验

1.1 用 **van-form** 包住
1.2 在 **van-field** 上要有 v-model=“变量” 和 绑定rules属性 **:rules="rules变量"**

```js
rules变量:[
	{
	// 是否必填
	required:true,
	message:错误信息,
	trigger:"onBlur/onChange"},
    {
    // 自定义表单校验
    validator: value => {
        // true:验证通过
        // false:验证不通过
        return boolean值
    },message:"错误信息",
    trigger:"onBlur/onChange"
    }
  ]
12345678910111213141516
```

## 2. 全局表单验证

2.1 在 van-form 中定义ref属性 **ref="xxx"**
2.2 在触发对应事件的函数中写入以下代码

```js
this.$refs.xxx.validate().then(()=>{
	// 验证通过
	}).catch(()=>{
	//验证失败
	})
12345
```

## 3. 局部表单验证

3.1 在 **van-form** 中定义ref属性 **ref="xxx"**
3.2 在需要验证的项的 **van-field**上加上 name值 **name="ooo"**
3.3 在触发对应事件的函数中写入以下代码

```js
this.$refs.xxx.validate("name的值").then(()=>{
	// 验证通过
	}).catch(()=>{
	//验证失败
	})
12345
```

# 三、完整示例代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">
    <title>vant表单验证</title>
</head>

<body>
    <div id="app">
        <van-form ref='form'>
            <!-- 手机号码 -->
            <van-field label="手机号码：" v-model='mobile' placeholder="请输入手机号码" :rules="telRules" name="mobile"></van-field>
            <!-- 验证码 -->
            <van-field label="验证码" v-model="code" placeholder="请输入验证码" :rules="codeRules">
                <!-- 通过 button 插槽可以在输入框尾部插入按钮 -->
                <template #button>
                    <!-- 这里有bug，使用<van-button>无法进行局部表单验证 -->
                    <!-- <van-button size="small" type="primary" @click="getCode">发送验证码</van-button> -->
                    <div class="button" @click="getCode">发送验证码</div>
                </template>
            </van-field>
            <van-button type="primary" block @click="submit">提交</van-button>
        </van-form>
    </div>

    <!-- 引入样式文件 -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/vant@2.12/lib/index.css" />
    <!-- 引入 Vue 和 Vant 的 JS 文件 -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6/dist/vue.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vant@2.12/lib/vant.min.js"></script>
    <script>
        // 在 #app 标签下渲染一个按钮组件
        new Vue({
            el: '#app',
            data: {
                mobile: '', // 手机号码
                code: '', // 验证码
                // 校验手机号码
                telRules: [{
                    required: true,
                    message: '手机号码不能为空',
                    trigger: 'onBlur'
                }, {
                	// 自定义校验规则
                    validator: value => {
                        return /^(0|86|17951)?(13[0-9]|15[012356789]|166|17[3678]|18[0-9]|14[57])[0-9]{8}$/
                            .test(value)
                    },
                    message: '请输入正确格式的手机号码',
                    trigger: 'onBlur'
                }],
                codeRules: [{
                    required: true,
                    message: '验证码不能为空',
                    trigger: 'onBlur'
                }]
            },
            methods: {
                getCode() {
                    // 局部表单验证
                    this.$refs.form.validate('mobile').then(() => {
                        this.$toast.success('验证码获取成功')
                    }).catch(() => {
                        this.$toast.fail('验证码获取失败')
                    })
                },
                submit() {
                    // 全局表单验证
                    this.$refs.form.validate().then(() => {
                        this.$toast.success('提交成功')
                    }).catch(() => {
                        this.$toast.fail('提交失败')
                    })
                }
            }
        });
    </script>
</body>
</html>
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081
```

# 四. 总结



| 属性                 | 功能           | 注意                                                    |
| -------------------- | -------------- | ------------------------------------------------------- |
| validator            | 自定义表单验证 | v-mode，绑定rules                                       |
| validate()           | 全局表单验证   | van-form加入属性ref=‘xxx’                               |
| validate(‘name的值’) | 局部表单验证   | van-form添加属性ref=‘xxx’，van-field 添加属性name=‘ooo’ |

