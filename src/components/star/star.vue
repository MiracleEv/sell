<template>
<span class="Fstar">
  <ul>
    <li v-for="item in starInit[0].star">
      <a><img :src="type.on"/></a>
    </li>
     <li v-for="item in starInit[1].blackStar">
       <a><img :src="type.off"/></a>
     </li>
  </ul>
</span>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      sellerScore: {},
      sellerService: {},
      sellerFood: {},
      ratingsData: {},
      starType: {}
    },
    data () {
      return {
        starInit: [
          {star: null},
          {blackStar: null}
        ],
        starData: {},
        type: {},
        Bigtype: {
          on: require('./star36_on@2x.png'),
          off: require('./star36_off@2x.png')
        },
        Smalltype: {
          on: require('./star24_on@2x.png'),
          off: require('./star24_off@2x.png')
        }
      };
    },
    created () {
      this.$http.get('/api/seller').then(response => {
        response = response.body;
        if (response.errno === 0) {
          // this.seller = response.data;
          // console.log(this.seller.score);
          this.getData(this.sellerScore, this.sellerService, this.sellerFood, this.ratingsData, this.starType);
        }
      });
    },
    methods: {
      getData (starScore, starService, starFood, ratingsData, starType) {
        // console.log(ratingsData);
        if (starScore) {
          this.starData = starScore;
        } else if (starService) {
          this.starData = starService;
        } else if (starFood) {
          this.starData = starFood;
        } else if (ratingsData) {
          this.starData = ratingsData;
        }
        if (starType === 'undefined' || !starType) {
            this.type = this.Smalltype;
        } else if (starType === 'big') {
          this.type = this.Bigtype;
        } else if (starType === 'small') {
          this.type = this.Smalltype;
        }
         // console.log(this.type);
        this.starInit = [
          {star: Math.floor(parseInt(Math.round(this.starData)))},
          {blackStar: 5 - Math.floor(parseInt(Math.round(this.starData)))}
        ];
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .Fstar
    display inline-block
    font-size 0
    & ul li
      display inline-block
      vertical-align top
      & a
        display block

</style>
