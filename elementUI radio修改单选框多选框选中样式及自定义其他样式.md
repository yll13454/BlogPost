##### 最近vue开发，遇到一个页面样式上的问题，单选框和多选框的样式显示问题，看下图片吧：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603121032765.png)
这是官方组件的显示效果，项目效果需求如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200603121225174.png)
找了很多资料，最后终于解决了，边看代码边解释吧：

```css
这里的.right是我自定义的类名，父盒子，我的单选组是直接放在这个盒子里面的，下面的写法是vue的scss写法，不懂得可以先去了解一下；
  .right{     
          width: 40%;
          border-bottom: 1px solid #cacaca;              
          /deep/{
            .el-radio{
              //从这里开始 就是更改颜色的代码，将你的颜色           改掉我的颜色就可以了
              .el-radio__label{
                color: #000 !important;
              }
              .el-radio__input{                               
                margin-bottom: px(5);
                &.is-checked {
                .el-radio__inner{
                    background-color:#28D4C1;
                    border-color:#28D4C1; 
                  }
                }
                .el-radio__inner{
                    &:hover{
                        border-color:#28D4C1;
                    }
                }
              }
            }   
          }
        }
```