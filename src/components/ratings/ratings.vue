<template>
  <div class="ratings">
    <div class="score_count">
      <div class="score">
        <p>{{seller.score}}</p>
        <p>综合评分</p>
        <p>高于周边商家<span>{{seller.rankRate}}%</span></p>
      </div>
      <div class="comment">
        <p>
          <span class="comment_server">服务态度</span>
          <star :sellerService="sellerService" :starType="this.starSmall"></star>
          <span class="serverScore">{{sellerService}}</span>
        </p>
        <p>
          <span class="comment_server">服务态度</span>
          <star :sellerFood="sellerFood" :starType="this.starSmall"></star>
          <span class="serverScore">{{sellerFood}}</span>
        </p>
        <p><span class="comment_server">送达时间</span><a>{{seller.deliveryTime}}分钟</a></p>
      </div>
    </div>

   <div class="price">
     <div class="priceCenter">
       <div class="ratingCount">
         <span>全部&nbsp;{{seller.ratingCount}}</span>
         <span>满意&nbsp;{{seller.minPrice}}</span>
         <span>不满意&nbsp;{{seller.deliveryPrice}}</span>
       </div>
       <div class="seePrice" @click="showPrice">
          <span class="icon-check_circle" ref="circle"></span>
          <span>只看有内容的评价</span>
       </div>
     </div>
    </div>
    <div class="review" ref="ratings_scroll">
      <ul>
        <li v-for="item in ratings" class="review_list">
          <div class="priceHeader">
            <div class="priceUser">
              <img :src="item.avatar" width="28" height="28"/>
            </div>
            <div class="priceCount">
              <div class="priceName">{{item.username}}</div>
              <div class="priceTime">
                <star class="starPadding" :ratingsData="item.score" :starType="starSmall"></star>
                <span v-if="item.deliveryTime">{{item.deliveryTime}}分钟送达</span>
              </div>
            </div>
            <div class="priceNow">
              <date :rateTime="item.rateTime"></date>
            </div>
          </div>
          <div class="priceCount">
            <div class="priceText">{{item.text}}</div>
            <div class="priceList">
              <div class="price_good" :class="item.rateType==0?'icon-thumb_up':'icon-thumb_down noGoodColor'"></div>
              <div class="recommend">
                <ul>
                  <li v-if="item.recommend" v-for="menu in item.recommend">
                    <a>{{menu}}</a>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import star from '../star/star';
  import date from '../date/date.vue';
  export default {
    props: {
      // seller: Object
    },
    data () {
      return {
        ratings: {},
        seller: {},
        sellerService: {},
        sellerFood: {},
        ratingsData: {},
        priceType: false,
        rateTime: {},
        starSmall: 'small'
      };
    },
    created () {
      this.$http.get('/api/ratings').then(response => {
        response = response.body;
        if (response.errno === 0) {
          this.ratings = response.data;
          // console.log(this.ratings);
          setTimeout(() => {
            this._initScroll();
          });
        }
      });
      this.$http.get('/api/seller').then(response => {
        response = response.body;
        if (response.errno === 0) {
          this.seller = response.data;
          this.sellerService = response.data.serviceScore;
          this.sellerFood = response.data.foodScore;
          // console.log(this.sellerService);
        }
      });
    },
    methods: {
      _initScroll () {
        this.ratings_scroll = new BScroll(this.$refs.ratings_scroll, {
          startX: 0,
          startY: 0,
          click: true,
          scrollY: true,
          preventDefault: true
          // scrollbar: true
        });
      },
      showPrice () {
        if (!this.priceType) {
          this.$refs.circle.style.color = '#00a0dc';
          this.priceType = true;
        } else {
          this.$refs.circle.style.color = '';
          this.priceType = false;
        }
      }
    },
    components: {
      star,
      date
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixing.styl"
  .score
    ratings-rightBorder()
    background-color #fff
    flex 1 137.5px
    display inline-block
    & p:nth-of-type(1)
      padding-top 18px
      font-size 24px
      color rgb(255,153,0)
      line-height 28px
      text-align center
    & p:nth-of-type(2)
      text-align center
      color rgb(7,17,27)
      font-size 12px
      line-height 12px
      padding-top 6px
      padding-bottom 8px
    & p:nth-of-type(3)
      font-size 10px
      text-align center
      line-height 10px
      padding-bottom 24px
      color #93999f
  .ratings
    background-color #f3f5f7
    height auto
    position absolute
    top 174px
    bottom 52px
    width 100%
    overflow hidden
    left: 0;
  .comment
    padding-top 18px
    padding-bottom 18px
    flex 1 237px
    font-size 0
    // height 112px
    background-color #fff
    display inline-block
    vertical-align top
    & p
      padding 0 12px 0 24px
      font-size 12px
      color rgb(7,17,27)
      line-height 18px
      display block
    & p:nth-of-type(2)
      padding 8px 12px 8px 24px
    & p:nth-of-type(3) a
      color #93999f
      font-size 12px
      padding-left 8px
    & p span
      vertical-align middle
  .score_count
    font-size 0px
    // background-color #fff
    // height 100%
    display flex
    border-1px(rgba(7,17,21,0.1))
  .price
    margin-top 18px
    topBorder()
    bootomBorder()
    background-color #fff
    display block
  .priceCenter
    display block
    margin 0 auto
    width 90%
  .ratingCount
    border-1px(rgba(7,17,21,0.1))
    padding  18px 0 18px 0
    & span
      padding 12px
      font-size 12px
      display inline-block
      margin-right 5px
      border-radius 2px
    & span:nth-of-type(1)
      background-color #00a0dc
      color #fff
    & span:nth-of-type(2)
      background-color #cfeffa
    & span:nth-of-type(3)
      background-color #eaebed
  .seePrice
    margin 18px 0 18px 0
    & span:nth-of-type(1)
      font-size 24px
      vertical-align bottom
      line-height 24px
      color #93999f
    & span:nth-of-type(2)
      font-size 16px
      line-height 24px
      vertical-align top
      color #93999f
  .review
    overflow hidden
    position absolute
    top 256px
    bottom 0px
    left 0
    width 100%
    background-color #fff
   .review_list
      width 90%
      margin 0 auto
      border-1px(rgba(7,17,21,0.1))
      &:last-child
        border-none()
  .priceHeader
    padding-top 18px
    position relative
  .priceUser
    display inline-block
    padding-right 12px
    & img
      border-radius 50%
  .priceName
    display inline-block
    vertical-align top
    font-size 10px
    color rgb(7,17,27)
    padding-bottom 4px
    line-height 12px
  .priceTime
    padding-bottom 6px
    & span
      font-size 10px
      font-weight 100
      color rgb(147,153,159)
      line-height 12px
  .priceCount
    display inline-block
    vertical-align top
  .starPadding
    padding-right 6px
  .priceText
    padding-left 44px
    padding-bottom 8px
    font-size 12px
    color rgb(7,17,27)
    line-height 18px
  .serverScore
    font-size 12px
    color rgb(255,153,0)
    line-height 18px
    padding-left 12px
  .comment_server
    padding-right 12px
  .priceTime span
    display inline-block
    vertical-align middle
  .priceList
    display block
    padding-left: 44px;
    padding-bottom: 8px;
    & > div
      display inline-block
      vertical-align middle
  .price_good
    font-size: 12px;
    color: rgb(0,160,220);
    line-height: 18px;
  .noGoodColor
    color rgb(183,187,191)
  .recommend
    overflow hidden
    text-overflow: ellipsis;
    white-space: nowrap;
    width 202px
    margin-left 4px
    & ul li
      width 48px
      display inline-block
      vertical-align middle
      border-width 1px
      border-style solid
      padding 0 6px 0 6px
      border-color rgba(7,17,27,0.1)
      border-radius 2px
      margin-right 8px
      background-color rgb(255,255,255)
      & a
        display block
        width 100%
        overflow hidden
        text-overflow ellipsis
        white-space nowrap
        text-align center
        font-size 9px
        color rgb(147,153,159)
        line-height 18px
  .priceNow
    margin-top 18px
    display inline-block
    position absolute
    top 0px
    right 0px
</style>
