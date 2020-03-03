<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <!-- 使用router-link组件来导航 -->
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <div class="content">
      <!-- 路由匹配到的组件将渲染在这里 -->
      <keep-alive>
        <router-view :seller="seller" ></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script>
import {urlParse} from './common/js/util'
import header from "./components/header/header";
const ERR_OK = 0;
export default {
  data() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse()
          console.log(queryParam)
          return queryParam.id
        })()
      }
    };
  },
  created() {
    this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
      response = response.body
      if(response.errno===ERR_OK) {
        // this.seller = response.data
        // 用于对象的合并 , Object.assign方法的第一个参数是目标对象，后面的参数都是源对象。
        this.seller = Object.assign({}, this.seller, response.data)
        console.log(this.seller.id)
      }else {
        console.log('no data')
      }
    });
  },
  components: {
    "v-header": header
  }
};
</script>
<style scoped lang="stylus">
@import './common/stylus/mixin.styl';
.tab
  display flex
  width 100%
  height 40px
  line-height 40px
  // border-bottom 1px solid rgba(7, 17, 27, 0.1)
  border-1px(rgba(7, 17, 27, 0.1));
 .tab-item 
    flex 1
    text-align: center;
    & > a
      display block
      font-size 14px
      color rgb(77, 85, 93)
      &.active
        color: rgb(240, 20, 20);
</style>
