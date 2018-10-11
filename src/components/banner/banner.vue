<template>
    <div class="pics_banner" ref="seller_scroll">
      <ul>
        <li v-for="item in seller.pics" class="pics">
          <a><img :src="item"/></a>
        </li>
      </ul>
    </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  export default {
    data () {
      return {
        seller: {},
        seller_scroll: {}
      };
    },
    created () {
      this.$http.get('/api/seller').then(response => {
        response = response.body;
        if (response.errno === 0) {
          this.seller = response.data;
        }
        setTimeout(() => {
          this._initScroll();
        });
      });
    },
    methods: {
      _initScroll () {
        this.seller_scroll = new BScroll(this.$refs.seller_scroll, {
          startX: 0,
          startY: 0,
          click: true,
          scrollX: true,
          preventDefault: true
        });
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .pics
    display inline-block
    width 120px
    height 90px
    margin-right 6px
    & a
      display block
      height 100%
      width 100%
      & img
        display block
        width 100%
        height 100%
  .pics_banner
    display inline-block
    overflow hidden
    & ul
      width 200%
</style>
