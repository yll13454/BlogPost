# vue实现两个区域滚动条同步滚动

[https://blog.csdn.net/qq_38519358/article/details/107343042](https://blog.csdn.net/qq_38519358/article/details/107343042)

![img](media/20200714175211780.gif)

其实主要是通过ref属性来操控两个div的scrollTop属性

```html
<div class="customer-span" ref="systemForm" @scroll="sysHandleScroll()"  @mouseover="changeFlag(false)">
  <div class="customer-span-form">
	<el-form label-suffix=":" class="form-static" label-position="right" label-width="100px">
        <el-form-item v-for="(item, index) in formItem" :key="index" :label="item.label">
			{{ systemFormData[item.model] }}
      </el-form-item>
    </el-form>
  </div>
</div>
<div class="customer-span" ref='externalForm' @scroll="exterHandleScroll()" @mouseover="changeFlag(true)">
  <div class="customer-span-form">    <el-form label-suffix=":" class="form-static" label-position="right" label- 
    width="100px">
      <el-form-item v-for="(item, index) in formItem" :key="index" :label="item.label">
        {{ externalFormData[item.model] }}
      </el-form-item>
     </el-form>
  </div>
</div>
```

js部分

```javascript
data() {
  return {
    flag: false
  }
},
method: {
  changeFlag(flag) {
    this.flag = flag
  },
  // 左右滚动条滚动同步
  sysHandleScroll() {
    if (!this.flag) {
      this.$refs.externalForm.scrollTop = this.$refs.systemForm.scrollTop
    }
  },
  exterHandleScroll() {
    if (this.flag) {
      this.$refs.systemForm.scrollTop = this.$refs.externalForm.scrollTop
    }
  }
}
```

步骤解析：

1、首先你要给你的两个div设置固定高度，分别出现滚动条

2、然后再通过给两个div分别绑定ref属性

3、接下来给两个div添加[scroll](https://so.csdn.net/so/search?q=scroll&spm=1001.2101.3001.7020)方法，监控滚动条变化

4、最后分别在方法里设置两个滚动条的scrollTop值一样

**解决滚轮滑动滚动条移动缓慢问题**

发生原因：因为对两个div都添加了scroll方法，一个区域滚动会改变另外一个区域的scrollTop,但是同时触发了这个区域自己的scroll方法，又会进行改变，这样就形成了两个scroll的无限回调，最终发生了看到的结果，移动非常缓慢。

解决思路：大致思路是添加一个flag属性，两个scroll事件分别传入不同的值，再根据这个值去判断是否设置scrollTop属性，这样就不会造成无线循环,这里的flag属性需要在scroll事件之前传入，所以这里通过mouserover事件传入该属性

```
      $("#div2").scrollTop($(this).scrollTop()); // 纵向滚动条
      $("#div2").scrollLeft($(this).scrollLeft()); // 横向滚动条
```

