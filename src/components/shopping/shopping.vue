<template>
  <div class="shopping">
    <div class="shope-wrapper">
      <div class="cost">
        <div class="big-circle">
          <div class="small-circle">
            <span class="car icon-shopping_cart"></span>
          </div>
        </div>
        <span class="money" id="money">&yen;0</span>
      </div>
      <div class="fee">
        <p class="fee-money">另需配送费&yen;{{shope.deliveryPrice}}元</p>
      </div>
    </div>
    <div class="payment-wrapper">
      &yen;{{shope.minPrice}}起送
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default{
    props: {
      shope: Object
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixing.styl"
  .shopping
    position fixed
    bottom 0px
    left 0px
    height 52px
    font-size 16px
    width 100%
  .shope-wrapper
    background-color #141d27
    height 100%
    width 100%
    display block
  .cost
    display inline-block
    margin-left 8px
    position relative
    height 100%
    width 104px
  .shope-car
    position relative
  .car
    font-size 24px
    color rgba(255,255,255,.4)
    line-height 40px
    display block
    text-align center
    height 100%
    vertical-align bottom
  .big-circle
    width 54px
    border-radius 50%
    height 54px
    position absolute
    background-color #141d27
    left 0px
    top -6px
    display inline-block
  .small-circle
    width 44px
    height 44px
    border-radius 50%
    position absolute
    top 6px
    background-color rgba(255,255,255,.4)  //  #00a0dc
    left 50%
    margin-left -22px
  .money
    margin-left 44px
    display inline-block
    margin-top 14px
    padding-right 12px
    height 24px
    line-height 24px
    color rgba(255,255,255,.4)
    font-weight 400
    width 60px
    text-align center
    shope-border(rgba(255,255,255,.1))
  .fee
    display inline-block
    margin-left 12px
    color rgba(255,255,255,.4)
    font-weight 400
    line-height 24px
    width 116px
  .payment-wrapper
    width 105px
    height 100%
    display block
    background-color rgba(255,255,255,0.4)
    position absolute
    right 0px
    bottom 0px
    color rgba(255,255,255,.4)
    font-weight 400
    line-height 52px
    text-align center
  iphone5()
</style>
