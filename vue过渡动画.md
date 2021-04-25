## vue中的transition组件

```html
<html>
    <header>
        <title>vue_test</title>
        <style>
            .cent{
                width: 100px;
                height: 100px;
                background-color: aqua;
            }
            .fade-enter-active{
                transition: all 5s;
            }
            .fade-leave-active{
                transition: all 3s;
            }
            .fade-enter, .fade-leave-to{
                transform: translate(500px);
                opacity: 0;
            }
        </style>
        <!-- 引入vue的js文件 -->
        <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    </header>
    <body>
        <!-- 定义一个ID为app的div，用于写script时挂载元素 -->
        <div id="app">
            <transition name='fade'>
            <div v-if='isshow' class="cent">
            </div>
            </transition>
            <button @click='changeshow'>切换状态</button>
          </div>
          <script>
            var app = new Vue({
            el: '#app',
            data: {
                isshow:true
            },
            methods:{
                changeshow:function(){
                    this.isshow=!this.isshow
                }
            }
            })
          </script>
    </body>
</html>
```

**通过transition对name进行绑定，自动追加class类名，然后进行CSS样式的追加，使其产生动画过渡效果。**

