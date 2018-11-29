<template>
  <span class="bubble" :class="widthType">{{num}}</span>
</template>

<script>
  export default {
  	name: 'bubble',
  	props: {
  	  num: {
  	  	type: Number,
  	  	default: 0
  	  },
      type: {
        type: Number,
        default: 1
      }
  	},
    computed: {
      widthType() {
        if (this.type === 1) {
          return 'cart'
        } else if (this.type === 2) {
          return 'slide'
        }
      }
    }
  }
</script>

<style lang="stylus" scoped>
  .bubble
    display: inline-block   
    height: 16px
    text-align: center
    line-height: 16px
    font-size: 9px
    font-weight: 700
    color: #fff
    background: rgb(240, 20, 20)
    &.cart
      width: 24px
      border-radius: 8px
    &.slide
      width: 16px
      border-radius: 50%
</style>
