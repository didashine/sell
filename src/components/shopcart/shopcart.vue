<template>
  <div class="shopcart">
    <div class="content">
      <div class="content-left">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount>0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalCount>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div class="pay" :class="payClass">
          {{payDesc}}
        </div>
      </div>
    </div>
  </div>

</template>

<script type="ecmascript-6">
  export default {
    props: {
      selectFoods:  {
        type: Array,
        default() {
          return [
            {
              price: 20,
              count: 2
            }
          ];
        }
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      },
    },
    computed: {
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          total += food.price*food.count;
        });
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`;
        } else if(this.totalPrice < this.minPrice){
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        }else {
          return '去支付';
        }
      },
      payClass() {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .shopcart
    position: fixed
    width: 100%
    left: 0
    bottom: 0
    z-index: 50
    height: 48px
    .content
      display:flex
      background: #141d27
      font-size: 0
      color:rgba(255,255,255,0.4)
      .content-left
        flex: 1
        .logo-wrapper
          display:inline-block
          position:relative
          top: -10px
          width: 56px
          height: 56px
          background: #141d27
          margin: 0 18px
          padding: 6px
          box-sizing: border-box
          border-radius:50%
          vertical-align: top
          .logo
            width: 44px
            height: 44px
            background: #2b343c
            border-radius:50%
            text-align:center
            &.highlight
              background: rgb(0,160,220)
            .icon-shopping_cart
              line-height: 44px
              font-size: 24px
              color: #80858a
              &.highlight
                color: #fff
          .num
            position:absolute
            right: 0
            top: 0
            width: 24px
            height: 16px
            text-align: center
            border-radius:16px
            line-height: 16px
            background: rgb(240,20,20)
            box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.4)
            font-size: 9px
            font-weight: 700
            color: #fff
        .price
          display:inline-block
          vertical-align:top
          margin-top: 12px
          line-height: 24px
          box-sizing:border-box
          padding-right: 12px
          border-right: 1px solid rgba(255,255,255,0.1)
          font-size: 16px
          font-weight:700
          &.highlight
            color: #fff
        .desc
          display: inline-block
          vertical-align: top
          margin: 12px 0 0 12px
          line-height: 24px
          font-size: 10px
      .content-right
        flex: 0 0 105px
        .pay
          height: 48px
          line-height: 48px
          text-align: center
          font-size: 12px
          font-weight: 700
          &.not-enough
            background: #2b333b
          &.enough
            background: #00b43c
            color: #fff
</style>
