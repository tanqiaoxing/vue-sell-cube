<template>
  <transition name="fade">
  	<cube-popup
      type="shop-cart-list"
      v-show="visible"
      :mask-closable=true
      :z-index=90
      position="bottom"
      @mask-click="maskClick"
  	>
  	  <transition 
  	    name="move"
        @after-leave="onLeave"
  	  >
  	  	<div v-show="visible">
  	  	  <div class="list-header">
  	  	  	<div class="cart-title">购物车</div>
  	  	  	<div class="cart-empty" @click="emptyCart">清空</div>
  	  	  </div>
  	  	  <cube-scroll class="list-content" ref="listContent">
  	  	  	<ul>
  	  	  	  <li class="food" v-for="food in selectFoods">
  	  	  	  	<div class="food-name">{{food.name}}</div>
  	  	  	  	<div class="food-price">
  	  	  	  	  <span class="symbol">¥</span>
  	  	  	  	  <span class="num">{{food.price}}</span>
  	  	  	  	</div>
  	  	  	  	<div class="cart-control-wrapper">
  	  	  	  	  <cart-control @add="onAdd" :food="food"></cart-control>
  	  	  	  	</div>
  	  	  	  </li>
  	  	  	</ul>
  	  	  </cube-scroll>
  	  	</div>
  	  </transition>
  	</cube-popup>
  </transition>
</template>

<script>
  import CartControl from 'components/cart-control/cart-control'
  import popupMixin from 'common/mixins/popup'

  const EVENT_LEAVE = 'leave'
  const EVENT_ADD = 'add'
   const EVENT_SHOW = 'show'

  export default {
    mixins: [popupMixin],
  	name: 'shop-cart-list',
  	props: {
  	  selectFoods: {
  	  	type: Array,
  	  	default() {
  	  	  return []
  	  	}
  	  }
  	},
    created() {
      this.$on(EVENT_SHOW,() => {
        this.$nextTick(() => {
          this.$refs.listContent.refresh()
        })
      })
    },
  	methods: {
  	  onLeave() {
  	  	this.$emit(EVENT_LEAVE)
  	  },
  	  maskClick() {
  	  	this.hide()
  	  },
  	  emptyCart() {
  	  	this.$createDialog({
          type: 'confirm',
          content: '清空购物车吗',
          $events: {
            confirm: () => {
              this.selectFoods.forEach((food) => {
                food.count = 0
              })
              this.hide()
            }
          }
        }).show()
  	  },
      onAdd(target) {
        this.$emit(EVENT_ADD, target)
      }
  	},
  	components: {
  	  CartControl
  	}
  }
</script>

<style lang="stylus" scoped>
  .cube-shop-cart-list
    bottom: 48px
    &.fade-enter-active, &.fade-leave-active
      transition: all .3s ease-in-out
    &.fade-enter, &.fade-leave-active
      opacity: 0
    .move-enter, .move-leave-active
      transform: translate3d(0, 100%, 0)
    .move-enter-active, .move-leave-active
      transition: all .3s ease-in-out
    .list-header
      height: 40px
      padding: 0 18px
      background: #f3f5f7
      border-bottom: 1px solid rgba(7, 17, 27, .1)
      .cart-title
        float: left
        line-height: 40px
        font-size: 14px
        font-weight: 200
        color: rgb(7, 17, 27)
      .cart-empty
        float: right
        line-height: 40px
        font-size: 12px
        color: rgb(0, 160, 220)
    .list-content
      max-height: 217px
      overflow: hidden
      padding: 0 18px
      background: #fff
      .food
        display: flex
        align-items: center
        width: 100%
        height: 48px
        border-bottom: 1px solid rgba(7, 17, 27, .1)
        .food-name
          flex: 1  
          line-height: 48px
          font-size: 14px
          color: rgb(7, 17, 27)
        .food-price
          margin-right: 12px
          color: rgb(240, 20, 20)
          .symbol
            font-size: 10px
          .num
            font-size: 14px
            font-weight: 700
</style>
