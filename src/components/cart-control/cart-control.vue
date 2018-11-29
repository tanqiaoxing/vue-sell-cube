<template>
  <div class="cart-control">
    <transition name="move">
      <div class="cart-decrease" @click.stop="decrease" v-show="food.count>0">
      	<span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop="add">
    </div>
  </div>
</template>

<script>
  const EVENT_ADD = 'add'

  export default {
  	name: 'cart-control',
  	props: {
      food: {
      	type: Object
      }
  	},
  	methods: {
  	  add(event) {
  	  	if(!this.food.count) {
          this.$set(this.food, 'count', 1)
  	  	} else {
  	  	  this.food.count++
  	  	}
        this.$emit(EVENT_ADD, event.target)
  	  },
  	  decrease() {
  	  	if (this.food.count) {
  	  	  this.food.count-- 
  	  	}
  	  }
  	}
  }
</script>

<style lang="stylus" scoped>
  .cart-control
    display: flex
    align-items: center
    .cart-decrease
      display: inline-block
      opacity: 1
      padding: 6px    
      .inner
        line-height: 24px    
        font-size: 24px
        color: rgb(0, 160, 220)
        transition: all .4s
        transform: rotate(0)
      &.move-enter-active, &.move-leave-active
        transition: all .4s
      &.move-enter, &.move-leave-to
        opacity: 0
        transform: translate3d(24px, 0, 0)
        .inner
          transform: rotate(180deg)
    .cart-count
      width: 12px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
</style> 












