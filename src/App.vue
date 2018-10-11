<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">平均</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
    <v-shopping :shope="seller"></v-shopping>
  </div>
</template>

<script>
  import header from './components/header/header.vue';
  import shopping from './components/shopping/shopping.vue';
  export default{
    data () {
      return {
        seller: {}
      };
    },
    created () {
      this.$http.get('/api/seller').then(response => {
        response = response.body;
        if (response.errno === 0) {
          this.seller = response.data;
          // console.log(this.seller);
        }
      });
    },
    components: {
      'v-header': header,
      'v-shopping': shopping
    }

  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "./common/stylus/mixing.styl"
    .tab
      display flex
      width 100%
      height 40px
      line-height 40px
      // border-bottom  1px solid rgba(7,17,21,0.1)   // border在移动设备dpi会＊2陪
      border-1px(rgba(7,17,21,0.1))
      .tab-item
        flex 1
        text-align center
        & > a
          display block
          font-size 14px
          color rgb(77,85,93)
          &.active
            color rgb(240,20,20)

</style>
