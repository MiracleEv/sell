<template>
  <div class="seller">
      <div class="seller_header">
        <div class="header_counter">
          <div class="seller_name">
            <h1>{{seller.name}}</h1>
            <div class="sellCount">
                <star :sellerScore="sellerScore"></star>
              <span class="rankRate">({{seller.rankRate}})</span>
              <span>月销{{seller.sellCount}}单</span>
              <span class="icon-favorite sellCount_icon" ref="favorite" @click="showFavorite"><p>已收藏</p></span>
            </div>

          </div>
          <div class="seller_price">
            <div class="seller_minPrice">
            <span>
              <p>起送价</p>
              <p>{{seller.minPrice}}<a class="minText">元</a></p>
            </span>
            </div>
            <div class="seller_deliveryPrice">
            <span>
              <p>商家配送</p>
              <p>{{seller.deliveryPrice}}<a class="minText">元</a></p>
            </span>
            </div>
            <div class="seller_deliveryTime">
            <span>
              <p>评价配送时间</p>
              <p>{{seller.deliveryTime}}<a class="minText">分钟</a></p>
            </span>
            </div>
          </div>
        </div>
      </div>
      <div class="scroll" ref="seller_scroll">
        <div class="seller_wrapper">
          <div class="seller_B">
            <div class="seller_count">
              <h1>公告与活动</h1>
              <p>{{seller.bulletin}}</p>
              <div class="seller_list">
                <supports></supports>
              </div>
            </div>
          </div>
          <div class="seller_banner">
            <div class="seller_count">
              <div class="look">
                <h1>商家实景</h1>
              </div>
              <div class="banner">
                <banner></banner>
              </div>
            </div>
          </div>
          <div class="seller_details">
            <div class="seller_count">
              <div class="info">
                <h1>商家信息</h1>
              </div>
            </div>
            <div class="seller_count info_list">
              <ul>
                <li v-for="item in seller.infos">
                  <a>{{item}}</a>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>


    </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star.vue';
  import BScroll from 'better-scroll';
  import supports from '../supports/supports.vue';
  import banner from '../banner/banner.vue';
  export default {
    data () {
      return {
        seller: {},
        sellerScore: {},
        detailShow: false
      };
    },
    created () {
      this.$http.get('/api/seller').then(response => {
        response = response.body;
        if (response.errno === 0) {
          this.seller = response.data;
          this.sellerScore = parseInt(response.data.score);
          setTimeout(() => {
            this._initScroll();
          });
        }
      });
    },
    components: {
      star,
      supports,
      banner
    },
    methods: {
      _initScroll () {
          this.seller_scroll = new BScroll(this.$refs.seller_scroll, {
              startX: 0,
              startY: 0,
              click: true,
              scrollY: true,
              preventDefault: true
             // bounce: true
          });
      },
      showFavorite () {
        if (this.detailShow) {
          this.$refs.favorite.style.color = '';
          this.detailShow = false;
        } else {
          this.$refs.favorite.style.color = '#f01414';
          this.detailShow = true;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixing.styl"
    .seller
      background-color #f3f5f7
      display block
    .seller_header
      background-color #fff
      width 100%
      padding-top 18px
      border-1px(rgba(7,17,27,0.1))
    .header_counter
      position relative
      width 90%
      margin 0 auto
    .seller_name
      display inline-block
      width 100%
      border-1px(rgba(7,17,27,0.1))
      padding-bottom 18px
      & h1
        font-size 14px
        color rgb(7,17,27)
        line-height 14px
        margin-bottom 8px
    .rankRate
      margin-right 12px
      margin-left 8px
    .sellCount
      & span
        font-size 10px
        line-height 18px
        color rgb(77,85,93)
        display inline-block
        &:last-child
          font-size 24px
          // color rgb(240,20,20)
          line-height 24px
          text-align center
          position absolute
    .sellCount_icon
      display block
      cursor: pointer;
      position absolute
      right 0;
      top 0
      & p
        font-size 10px
        color rgb(77,85,93)
    .seller_price
      display flex
      justify-content center
      & div
        flex 1
        margin-top 18px
        & span
          display block
          seller-rightBorder(38px)
        & span:last-child:after
          content none
        & span p:first-child
          text-align center
          font-size 10px
          color rgb(147,153,159)
          line-height 10px
          margin-bottom 4px
        & span p:last-child
          font-size 24px
          font-weight 100
          color rgb(7,17,27)
          line-height 24px
          text-align center
          margin-bottom 18px
          padding-top 4px
    .scroll
      overflow: hidden;
      position: absolute;
      top: 330px;
      bottom: 0px;
      background #f3f5f7
      left: 0;
      display block
      width: 100%;
    .seller_wrapper
      margin-top 18px
      padding-bottom 100px
      topBorder();
    .seller_count
      width 80%
      margin 0 auto
      padding-top 18px
      & h1
        margin-bottom 8px
      & p
        padding 0 12px
        font-size 12px
        font-weight 100
        color rgb(240,20,20)
        line-height 24px
        padding-bottom 16px
        bootomBorder()
  .seller_list
    & ul li
      margin-top 12px
      padding-bottom 12px
      border-bottom 1px solid rgba(7,17,27,0.1)
    & ul li:last-child
      border none
    & ul li a
      color rgb(7,17,27)
      font-weight 100
      line-height 16px
      font-size 12px
  .seller
    height auto
  .seller_B
    background-color #fff
    bootomBorder();
    margin-bottom 16px
  .seller_banner
    background-color #fff
    topBorder();
    bootomBorder();
  .info_list
    padding-top 12px !important
    & li a
      font-size 12px
      font-weight 100
      color rgb(7,17,27)
      line-height 16px
      display block
      border-top 1px solid rgba(7,17,27,0.1)
      padding 16px 0 16px 12px
  .seller_details
    background #fff
    margin-top 16px
  .banner
    padding-bottom 18px
  .minText
    font-size 10px

</style>
