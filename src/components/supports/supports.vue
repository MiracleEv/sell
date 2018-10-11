<template>
  <div class="supports_menu">
    <ul>
      <li v-for="item in seller.supports" class="supports_list">
        <a class="supports_img"><span :class="supportsMenu[item.type]" class="icon_5"></span>{{item.description}}</a>
      </li>
    </ul>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
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
      this.supportsMenu = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .icon_5
    display block
    width 18px
    height 18px
  .supports_menu
    margin-bottom 28px
  .supports_list
    margin-bottom 12px
  .supports_img
    font-size 12px
    font-weight 100
    color rgb(255,255,255)
    line-height 12px
    & span
      display inline-block
      margin-right 6px
      margin-left 12px
      vertical-align middle
</style>
