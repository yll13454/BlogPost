一，安装swiper

执行命令 npm install vue-awesome-swiper --save

二，引入swiper

import {Swiper} from "vue-awesome-swiper";
import "swiper/dist/css/swiper.css";

 

三，使用swiper，不废话，上代码。

```vue
<template>
  <div class="page">
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide">
          <img class="imgCard" src="../assets/swiper1.jpg" alt>
        </div>
        <div class="swiper-slide">
          <img class="imgCard" src="../assets/swiper2.jpg" alt>
        </div>
        <div class="swiper-slide">
          <img class="imgCard" src="../assets/swiper3.jpg" alt>
        </div>
      </div>
      <div class="swiper-pagination"></div>
    </div>
  </div>
</template>
<script>
import Swiper from "swiper";
import "swiper/dist/css/swiper.css";
export default {
  data() {
    return {
      dialogShow: false
    };
  },
  mounted() {
    this._initSwiper();
  },
  methods: {
    _initSwiper() {
      var mySwiper = new Swiper(".swiper-container", {
        direction: "horizontal",
        loop: true,
        autoplay: true, //自动轮播
        speed: 1000,
        pagination: {
          el: ".swiper-pagination",
          type: "custom",
          renderCustom: function(swiper, current, total) {
            var customPaginationHtml = "";
            for (var i = 0; i < total; i++) {
              //判断哪个分页器此刻应该被激活
              if (i == current - 1) {
                customPaginationHtml +=
                  '<span class="swiper-pagination-customs swiper-pagination-customs-active"></span>';
              } else {
                customPaginationHtml +=
                  '<span class="swiper-pagination-customs"></span>';
              }
            }
            return '<span class="swiperPag">'+customPaginationHtml+'</span>';
          }
        }
      });
    }
  }
};
</script>
<style lang="scss" >
.swiperPag {
  width:4.5rem;
  height: 0.07rem;
  border-radius: 0.04rem;
  display: flex;
  align-items: center;
  margin:0 auto;
  background-color: rgba($color: #000000, $alpha: 0.8)
}
.swiper-pagination-fraction, .swiper-pagination-custom, .swiper-container-horizontal > .swiper-pagination-bullets {
  bottom:0.27rem;
}
.swiper-pagination-customs {
  width: 1.5rem;
  height: 0.14rem;
  display: inline-block;
}
/*自定义分页器激活时的样式表现*/
.swiper-pagination-customs-active {
  width: 1.5rem;
  height: 0.14rem;
  display: inline-block;
  border-radius: 0.07rem;
  background-color: #28a7e1;
}
</style>
```

注意：

- style标签不要加scoped,否则样式加不上！
- 直接npm install swiper --save 下载的是swiper4，build打包时，会报错如下： Unexpected token: name (Dom7) [./~/swiper/~/dom7/dist/dom7.modular.js:16,0][static/js/vendor.cf492f2bb7f8b02ec428.js:16311,6]

 到后来才发现，这样写是有问题的，当路由切换后再次进入该页面轮播就停止了，然后就做了如下更改。

```js
export default {
  data() {
    return {
      dialogShow: false,
      mySwiper: {},
    };
  },
  activated() {
    this._initSwiper(); // 初始化swiper
  },
  deactivated() {
      this.mySwiper.destroy();// 销毁swiper
  },
  methods: {
    _initSwiper() {
      this.mySwiper = new Swiper(".swiper-container", {
        direction: "horizontal",
        loop: true,
        autoplay: true, //自动轮播
        speed: 1000,
        pagination: {
          el: ".swiper-pagination",
          type: "custom",
          renderCustom: function(swiper, current, total) {
            var customPaginationHtml = "";
            for (var i = 0; i < total; i++) {
              //判断哪个分页器此刻应该被激活
              if (i == current - 1) {
                customPaginationHtml +=
                  '<span class="swiper-pagination-customs swiper-pagination-customs-active"></span>';
              } else {
                customPaginationHtml +=
                  '<span class="swiper-pagination-customs"></span>';
              }
            }
            return '<span class="swiperPag">'+customPaginationHtml+'</span>';
          }
        }
      });
    }
  }
};    
```

 