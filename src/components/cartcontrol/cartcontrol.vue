<template>
  <div class="cartontrol">
    <div class="cart-decrease icon-remove_circle_outline" v-show="this.food.count>0" @click="decreaseCart"></div>
    <div class="cart-count" v-show="this.food.count>0">{{this.food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default{
    props: {
      food: {
        type: Object
      }
    },
    created () {
      // console.log(this.food.count);
    },
    methods: {
      addCart (event) {
        if (!event._constructed) {    // 阻止重复点击
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1); // this.food.count在vue全局中得用set
         // this.food.count = 1;
        } else {
          this.food.count++;
        }
      },
      decreaseCart (event) {
        if (!event._constructed) {    // 阻止重复点击
          return;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1); // this.food.count在vue全局中得用set
          // this.food.count = 1;
        } else {
          this.food.count--;
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartontrol
    .cart-decrease,.cart-add
      display inline-block
      line-height 24px
      font-size 24px
      color rgb(0,160,220)
    .cart-count
      font-size 10px
      color rgb(147,153,159)
      line-height 24px
      vertical-align top
      text-align center
      display inline-block
</style>
