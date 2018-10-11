<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="64" height="64"/>
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="supports">
          <span class="icon_1" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>

      </div>
      <div v-if="seller.supports" class="number" @click="showDetail">
        <span class="len">{{seller.supports.length}}个</span>
        <span class="icon_2 icon-keyboard_arrow_right"></span>
      </div>
    </div>
    <div v-if="seller.supports" class="bulletin-wrapper" @click="showDetail">
      <span class="icon_3" :class="classMap[seller.supports[1].type]"></span>
      <div class="bulletin_text">{{seller.bulletin}}</div>
      <span class="icon_4 icon-keyboard_arrow_right"></span>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%"/>
    </div>
    <div v-show="detailShow" class="detail">
        <div class="detail_title">{{seller.name}}</div>
        <div class="star">
          <star :ratingsData="seller.score" :starType="this.starBig"></star>
        </div>
      <div class="seller_header_area">
        <div class="supports_detail">优惠信息</div>
        <div class="supports_menu">
          <supports></supports>
        </div>
        <div class="supports_detail">商家公告</div>
        <div class="supports_text">{{seller.bulletin}}</div>
      </div>
      <div class="supports_blur"></div>
      <span class="icon-close close_icon" @click="hideDetail"></span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star.vue';
  import supports from '../supports/supports';
  export default{
    props: {
      seller: Object
    },
    data () {
      return {
        detailShow: false,
        ratingsData: {},
        starBig: 'big'
      };
    },
    methods: {
      showDetail () {
        this.detailShow = true;
      },
      hideDetail () {
        this.detailShow = false;
      }
    },
    created () {
      this.classMap = ['decrease', 'bulletin', 'discount', 'guarantee', 'invoice', 'special'];
    },
    components: {
      star,
      supports
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixing.styl"
  .header
    color: #fff
    background-color rgba(7,17,27,.5)
    position relative
    overflow hidden
  .content-wrapper
    padding 24px 12px 18px 24px
    font-size 0
    display block
    height 64px
    position relative

  .avatar,.content
    display inline-block
    float: left
  .content
    font-size 14px
    margin-left 16px
    height 64px

  .title
    margin 2px 0px 8px 0px
  .brand
    display inline-block
    width 30px
    height 18px
    bg-img('brand')
    vertical-align top
  .name
    margin-left 6px
    line-height 18px
    font-weight bold
    font-size 16px
  .description
    margin-bottom 10px
    font-size 12px
    font-weight 100
    line-height 12px
  .icon_1
    width 12px
    height 12px
    display inline-block
    margin 0 4px 0 2px
  &.decrease
    bg-img('decrease_1')
  &.bulletin
    bg-img('bulletin')
  &.discount
    bg-img('discount_1')
  &.guarantee
    bg-img('guarantee_1')
  &.invoice
    bg-img('invoice_1')
  &.special
    bg-img('special_1')
  .text
    font-size 10px
    font-weight 100
    line-height 12px
  .number
    position: absolute
    display: inline-block
    background-color rgba(255,255,255,.2)
    border-radius 50px
    height 24px
    right 7px
    bottom 18px
  .len
    font-size 10px
    display inline-block
    color #fff
    font-weight 100
    line-height 12px
    margin-left 8px
    margin-top 6px
  .icon_2
    vertical-align bottom
    font-size 10px
    display inline-block
    color #fff
    font-weight 100
    line-height 12px
    margin 0 8px 0 2px

  .bulletin-wrapper
    height 28px
    background-color rgba(7,17,27,.2)
    line-height 28px
    position: relative
  .bulletin_text
    color #fff
    font-size 10px
    font-weight 100
    height 100%
    display block
    float left
    overflow hidden
    text-overflow ellipsis
    white-space nowrap
    width 80%
  .icon_3
    width 22px
    vertical-align bottom
    height 12px
    float left
    display inline-block
    margin 8px 4px 0px 12px
  .icon_4
    display block
    line-height 28px
    position absolute
    right 12px
    top 0
  .background
    position absolute
    overflow hidden
    top 0
    left 0
    z-index -1
    height 100%
    width 100%
    filter blur(10px)
  .detail
    position fixed
    height 100%
    width 100%
    top 0
    left 0
    background-color rgba(7,17,27,.8)
    overflow auto
    z-index 100
  .detail_title
    text-align center
    font-size 16px
    font-weight 700
    line-height 16px
    color: #fff
    padding 64px 0px 16px 0px
  .star
    height 24px
    margin-top 16px
    margin-bottom 28px
  .star > span
    display block
    margin 0 auto
    width 250px
    padding-right 20px
    & ul li
      margin-left 20px
  .seller_header_area
    display block
    width 80%
    margin 0 auto
  .supports_detail
    text-align center
    font-size 14px
    font-weight 350
    margin-bottom 24px
    color rgb(255,255,255)
    line-height 12px
    supports_border()
  .supports_text
    width 90%
    margin 0 auto
    font-size 12px
    font-weight 100
    line-height 24px
    color rgb(255,255,255)
  .supports_blur
    position fixed
    bottom 0px
    height 10%
    width 100%
    filter blur(10px)
    background rgba(7,17,27,0.8)
  .close_icon
    position fixed
    z-index 9999
    font-size 32px
    color rgba(255,255,255,0.5)
    bottom 32px
    left 50%
    margin-left -16px
</style>
