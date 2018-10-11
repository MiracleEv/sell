<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menu_scroll">
      <ul>
        <li v-for="(item,index) in goods" @click="jump(index)" class="menu">
          <a>
            <span class="goods_text border-1px">
            <span v-show="item.type>0" class="goods_icon_1" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
          </a>
        </li>
      </ul>
    </div>
    <div class="goods-wrapper" ref="goods_scroll">
      <ul>
        <li v-for="item in goods" class="food_list">
          <h1>
            <a>{{item.name}}</a>
          </h1>
          <ul>
            <li v-for="food in item.foods" class="food_item">
              <div class="food_icon">
                <img :src="food.icon" width="64" height="64"/>
              </div>
              <div class="food_content">
                <h2 class="food_name">{{food.name}}</h2>
                <p class="food_desc">{{food.description}}</p>
                <div class="food_extra">
                  <span>月销{{food.sellCount}}</span>
                  <span>好评率{{food.rating}}</span>
                </div>
                <div class="food_price">
                  <span class="food_price_new"><a>&yen;</a><a>{{food.price}}</a></span>
                  <span class="food_price_old" v-show="food.oldPrice">&yen;{{food.oldPrice}}</span>
                </div>
              </div>
              <!--
              <div class="addTo">
                <span class="del icon-remove_circle_outline" @click="delClick($event)" v-show=""></span>
                <span class="addText"></span>
                <span class="add icon-add_circle" @click="addClick($event)"></span>
              </div>
              -->
              <div class="cartcontrol-warpper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  // import dataJson from '../../../data.json';
    import BScroll from 'better-scroll';
    import cartcontrol from '../cartcontrol/cartcontrol.vue';
  export default {
    data () {
      return {
        goods: {},
        scrollData: [],
        menuData: []
      };
    },
    mounted () {}, // _initScroll放mounted导致bug 滚动条撑满，页面不滚动
    methods: {
      // let scroll = new BScroll('.goods-wrapper');
      _initScroll () {
        this.goods_scroll = new BScroll(this.$refs.goods_scroll, {
          startX: 0,
          startY: 0,
          click: true,
          scrollY: true,
          preventDefault: true,
          scrollbar: true,
          probeType: 3
        });
        this.menu_scroll = new BScroll(this.$refs.menu_scroll, {
          startX: 0,
          startY: 0,
          click: true,
          scrollY: true,
          preventDefault: true
        });
      },
      jump (index) {
        let jump = document.getElementsByClassName('food_list');
        let menu = document.getElementsByClassName('menu');
        for (let i = 0; i < menu.length; i++) {
          menu[i].style.background = '';
        }
        menu[index].style.background = '#fff';
        let total = -jump[index].offsetTop;
        this.goods_scroll.scrollTo(0, total, 1000);
       // console.log(total);
      },
      setScrollData () {
       let jump = document.getElementsByClassName('food_list');
       let menu = document.getElementsByClassName('menu');
        for (let i = 0; i < jump.length; i++) {
          this.scrollData[i] = -jump[i].offsetTop;
        }
        for (let s = 0; s < jump.length; s++) {
          this.menuData[s] = -menu[s].offsetTop;
        }
        console.log(this.scrollData);
        this.goods_scroll.on('scroll', (pos) => {
           // console.log(Math.floor(pos.y));
          if (this.goods_scroll.directionY === 1) {
            for (let j = 0; j < this.scrollData.length; j++) {
              if (Math.floor(pos.y) >= this.scrollData[j]) {
                // alert(j);
                menu[j].style.background = '#fff';
                // alert(this.menuData[j]);
                this.menu_scroll.scrollTo(0, this.menuData[j], 1000);
                break;
              } else {
                menu[j].style.background = '';
              }
            }
          } else if (this.goods_scroll.directionY === -1) {
            for (let k = this.scrollData.length - 1; k >= 0; k--) {
              if (Math.floor(pos.y) <= this.scrollData[k]) {
                // alert(k);
                // console.log(Math.floor(pos.y));
                menu[k].style.background = '#fff';
                this.menu_scroll.scrollTo(0, this.menuData[k], 1000);
                break;
              } else {
                // alert(k);
                menu[k].style.background = '';
              }
            }
          }
        });
      }
      /*,
      addClick ($event) {
       let addText = $event.target.parentNode.children[1].innerHTML;
       let sum = $event.target.parentNode.parentNode.children[1].children[3].children[0].children[1].innerHTML;
       let money = document.getElementById('money');
       if (addText === '') {
         addText++;
         $event.target.parentNode.children[1].innerHTML = 1;
         $event.target.parentNode.children[0].style.display = '';
       } else {
         addText++;
         $event.target.parentNode.children[1].innerHTML = addText;
       }
        // this.showDel = true;
        // let oldSum = parseInt($event.target.parentNode.parentNode.children[1].children[3].children[0].children[1].innerHTML);
        money.innerHTML = '&yen' + (sum * addText);
      },
      delClick ($event) {
        let addText = $event.target.parentNode.children[1].innerHTML;
        if (addText === '') {
          $event.target.parentNode.children[1].innerHTML = 0;
        } else if (addText === '1') {
          $event.target.parentNode.children[1].innerHTML = '';
          $event.target.parentNode.children[0].style.display = 'none';
        } else {
          addText--;
          $event.target.parentNode.children[1].innerHTML = addText;
        }
      } */
    },
    created () {
      this.$http.get('/api/goods').then(response => {
      response = response.body;
      if (response.errno === 0) {
        this.goods = response.data;
        // console.log(this.goods);
        setTimeout(() => {
          this._initScroll(); // ajax后进行回调更新
          this.setScrollData();
        });
      }
    });
      this.classMap = [ 'special', 'decrease', 'discount', 'bulletin', 'guarantee', 'invoice' ];
    },
    components: {
      cartcontrol
    }
  };

</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixing.styl"
  .goods
    position absolute
    top 174px
    bottom 52px
    width 100%
    overflow hidden
    left 0
    display flex
  .menu-wrapper
    flex 0 80px
    width 80px
    background-color #f3f5f7
    & ul li
      height 54px
      line-height 56px
      display table
      width 100%
      & a
        display block
        height 100%
        line-height 54px
        width 100%
      goods-border(56px, 10px)
    & ul li:last-child:after
      display none
  .goods-wrapper
    // overflow-y: auto
    flex 1
  .goods-wrapper > ul
    display block
    height auto
  .goods_icon_1
    vertical-align top
    display inline-block
    width 12px
    height 12px
   // margin-right 2px
  .goods_text
    display inline-block
    font-size 12px
    line-height 14px
    width 56px
    text-align center
    color: #000
    // color: rgb(240,20,20)
    padding 0px 12px 0 12px
    vertical-align middle
  .food_list h1
    font-size 12px
    color rgb(147,153,159)
    line-height 26px
    background-color #f3f5f7
    padding-left 14px
    border-left 4px solid #d9dde1
    & a
      display block
  .food_item
    background-color #fff
    width auto
    display block
    goods-border(88%, 18px)
    &:last-child
      border-none()
  .food_icon
    padding 18px 0px 0px 18px
    display inline-block
  .food_content
    display inline-block
    vertical-align top
    width 115px
    padding 20px 0px 18px 10px
  .food_name
    padding-bottom 8px
    font-size 14px
    color rgb(7,17,27)
    line-height 14px
  .food_desc
    padding-bottom 8px
    font-size 10px
    color rgb(147,153,159)
    line-height 10px
    text-overflow ellipsis
    white-space nowrap
    width 100%
    overflow hidden
  .food_extra
    padding-bottom 8px
    font-size 10px
    color rgb(147,153,159)
    line-height 10px
  .food_price_new
    font-size 14px
    color #ff0000
    // color rgb(147,153,159)
    font-weight 700
    line-height 24px
    & a
      font-size 14px
      color #ff0000
      // color rgb(147,153,159)
      font-weight 700
      line-height 24px
  .food_price_old
    font-size 10px
    color rgb(147,153,159)
    font-weight normal
    line-height 24px
  .addTo
    display inline-block
    position: absolute
    right 18px
    top 78px
    width 80px
  .add
    font-size 24px
    color rgb(0,160,220)
    line-height 24px
    float right
  .del
    font-size 24px
    color rgb(0,160,220)
    line-height 24px
    display inline-block
  .addText
    font-size 10px
    color rgb(147,153,159)
    line-height 24px
    vertical-align top
    text-align center
    display inline-block
    width 24px
  .cartcontrol-warpper
    display inline-block
    position: absolute
    right 18px
    top 78px
  .scrollDisplay
    background rgba(243,245,247,0.5)
</style>
