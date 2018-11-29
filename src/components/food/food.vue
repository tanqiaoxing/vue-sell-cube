<template>
  <transition 
    name="move"
    @after-leave="afterLeave"
  >
  	<div class="food" v-show="visible">
      <cube-scroll ref="scroll" :data="filteredRatings">
  		  <div class="icon">
  		    <img :src="food.icon" />
          <div class="back" @click="hide">
            <i class="icon-arrow_lift"></i>
          </div>
  		  </div>      
  		  <div class="content">
  		    <div class="name">{{food.name}}</div>
  		    <div class="detail">
  		  	  <span class="sell-count">月售{{food.sellCount}}</span>
  		      <span>好评率{{food.rating}}%</span>
  		    </div>
  		    <div class="price">
  		  	  <span class="now">¥{{food.price}}</span>
  		  	  <span v-show="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
  		    </div>
  		    <div class="cart-control-wrapper" v-show="food.count>0">
  		  	   <cart-control :food="food" @add="addFood"></cart-control>
  		    </div>
  		    <transition name="fade">
  		  	  <div @click="addCart" class="add-cart" v-show="!food.count">加入购物车</div>
  		    </transition>	  
  		  </div>
  		  <split></split>
        <div class="desc" v-show="food.description">
          <div class="title">商品介绍</div>
          <p class="desc-content">{{food.info?food.info:'暂无介绍'}}</p>
        </div>
        <split v-show="food.description"></split>
        <div class="ratings">
          <div class='title'>商品评价</div>
          <div class="rating-select-wrapper">
            <rating-select
              @select="onSelect"
              @toggle="onToggle"
              :ratings="ratings"
              :desc="desc"
              :selectType="selectType"
              :onlyContent="onlyContent"
            >       
            </rating-select>
          </div>         
          <ul v-show="ratings && ratings.length">
            <li class="rating" v-for="rating in filteredRatings">
              <div class="rating-info">
                <div class="time">{{format(rating.rateTime)}}</div>
                <div>{{rating.username}}</div>
                <div class="avatar">
                  <img :src="rating.avatar" />
                </div>
              </div>
              <div class="rating-text">
                <div class="icon-thumb">
                  <i :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></i>
                </div>
                <p class="text">{{rating.text}}</p>
              </div>
            </li>
          </ul>
        </div>
  	  </cube-scroll>
  	</div> 
  </transition>
</template>

<script>
  import CartControl from 'components/cart-control/cart-control'
  import Split from 'components/split/split'
  import RatingSelect from 'components/rating-select/rating-select'
  // import ShopCartSticky from 'components/shop-cart-sticky'
  // import RatingSelect from 'components/rating-select/rating-select'
  import popupMixin from 'common/mixins/popup'
  import moment from 'moment'
  import ratingMixin from 'common/mixins/rating'

  const ALL = 2

  const EVENT_SHOW = 'show'
  const EVENT_LEAVE = 'leave'
  const EVENT_ADD = 'add'

  export default {
  	mixins: [popupMixin,ratingMixin],
    name: 'food',
    props: {
      food: {
      	type: Object,
      	default: {}
      }
    },
    data() {
      return {
      	desc: {
      	  all: '全部',
      	  positive: '推荐',
      	  negative: '吐槽'
      	}
      }
    },
    created() {
      this.$on(EVENT_SHOW,() => {
        this.$nextTick(() => {
          this.$refs.scroll.refresh()
        })
      })
    },
    computed: {
      ratings() {
      	return this.food.ratings
      }
    },
    methods: {
      addCart(event) {
        this.$set(this.food, 'count', 1)
        this.$emit(EVENT_ADD, event.target)
      },
      addFood(target) {
        this.$emit(EVENT_ADD, target)
      },
      afterLeave() {
        this.$emit(EVENT_LEAVE)
      },
      format(time) {
        return moment(time).format('YYYY-MM-DD hh:mm')
      }
    },
    components: {
      CartControl,
      Split,
      RatingSelect
      // RatingSelect
    }
  }
</script>

<style lang="stylus" scoped>
  .food
    position: fixed
    top: 0
    bottom: 48px
    z-index: 30
    width: 100%
    background: #fff
    &.move-enter-active, &.move-leave-active
      transition: all .3s linear
    &.move-enter, &.move-leave-active
      transform: translate3d(100%, 0, 0)
    .icon
      position: relative
      width: 100%
      height: 0
      padding-top: 100%
      img
        position: absolute
        top: 0
        left: 0
        width: 100%
        height: 100%
    .back
      position: absolute
      top: 20px
      left: 15px
      color: #fff
    .content
      position: relative
      padding: 18px
      .name
        line-height: 14px
        font-size: 14px
        font-weight: 700
        color: rgb(7, 17, 27)
      .detail
        margin-top: 8px
        line-height: 19px
        font-size: 10px
        color: rgb(147, 153, 159)
        .sell-count
          margin-right: 12px
      .price
        margin-top: 18px
        .now
          margin-right: 10px
          line-height: 24px
          font-size: 14px
          font-weight: 700
          color: rgb(240, 20, 20)
        .old
          font-size: 10px
          font-weight: 700
          color: rgb(147, 153, 159)
      .add-cart
        position: absolute
        right: 18px
        bottom: 18px
        width: 72px
        height: 24px
        line-height: 24px
        color: #fff
        font-size: 10px
        text-align: center
        background: rgb(0, 160, 220)
        border-radius: 12px
        opacity: 1
        &.fade-enter-active, &.fade-leave-active
          transition: all .3s
        &.fade-enter, &.fade-leave-active
          opacity: 0
          z-index: -1      
      .cart-control-wrapper
        position: absolute
        right: 12px
        bottom: 12px
    .desc
      position: relative
      padding: 18px
      .title
        margin-bottom: 6px
        font-size: 14px
        font-weight: 700
      .desc-content
        line-height: 24px
        font-weight: 200
        font-size: 12px
        color: rgb(77, 85, 93)
    .ratings
      padding-top: 18px
      .title
        padding: 0 18px
        font-size: 14px
        font-weight: 700
      .rating-select-wrapper
        padding: 0 18px
        border-bottom: 1px solid rgba(7, 17, 27, .1)
      .rating
        margin: 0 18px
        padding: 16px 0
        border-bottom: 1px solid rgba(7, 17, 27, .1)
        &:last-child
          border-bottom: none
        .rating-info
          display: flex
          margin-bottom: 6px
          line-height: 12px
          font-size: 10px
          color: rgb(147, 153, 159)
          .time
            flex: 1
          .avatar
            width: 12px
            height: 12px
            margin-left: 6px          
            img
              width: 12px
              height: 12px
              border-radius: 50%
        .rating-text
          display: flex
          .icon-thumb
            margin-right: 4px
            line-height: 16px
            font-size: 12px
            .icon-thumb_up
              color: rgb(0, 160, 220)
            .icon-thumb_down
              color: rgb(147, 153, 159)
          .text
            line-height: 16px
            font-size: 12px
            color: rgb(7, 17, 27)
</style>

























