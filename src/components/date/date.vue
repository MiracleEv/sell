<template>
  <div class="rateTime">{{rateTime | formatDate}}</div>
</template>

<script type="text/ecmascript-6">
  import {formatDate} from './setDate.js';
  export default {
    props: {
      rateTime: {}
    },
    filters: {
      formatDate (time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh : mm');
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .rateTime
    font-size 10px
    font-weight 100
    color rgb(147,153,159)
    line-height 12px
</style>
