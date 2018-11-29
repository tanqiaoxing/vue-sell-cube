<template>
  <cube-scroll :options="scrollOptions" :data="filteredRatings">
    <div class="ratings" >
      <div class="overall">
        <div class="overall-left">
          <div class="score">{{seller.score}}</div>
          <div class="text">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overall-right">
          <div class="service-score right-item">
            <div class="text">服务态度</div>
            <div class="star-wrapper">
              <star :size=36 :score="seller.serviceScore"></star>
            </div>
            <div class="right-score">{{seller.serviceScore}}</div>
          </div>
          <div class="food-score right-item">
            <div class="text">商品评价</div>
            <div class="star-wrapper">
              <star :size=36 :score="seller.foodScore"></star>
            </div>
            <div class="right-score">{{seller.foodScore}}</div>
          </div>
          <div class="delivery-time right-item">
            <div class="text">送达时间</div>
            <div class="time">{{seller.deliveryTime}}分钟</div>
          </div>
        </div>
      </div>
      <split></split>
      <div class="rating-select-wrapper" v-if="ratings.length">
        <rating-select
          @select="onSelect"
          @toggle="onToggle"
          :ratings="ratings"
          :selectType="selectType"
          :onlyContent="onlyContent"
        ></rating-select>
      </div>
      <ul>
        <li class="ratings-list" v-for="(rating, index) in filteredRatings" :key="index">
          <div class="rating-user">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar" />
            </div>
            <div class="info">
              <p class="name">{{rating.username}}</p>
              <div class="score-time">
                <div class="star-wrapper">
                  <star :size=24 :score="rating.score"></star>
                </div>                
                <span class="delivery-time">{{rating.deliveryTime}}分钟送达</span>
              </div>
            </div>
            <div class="rate-time">{{format(rating.rateTime)}}</div>
          </div>
          <div class="content">
             <p class="text">{{rating.text}}</p>
             <div class="recommend">
              <i class="icon-thumb" :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></i>
              <span class="recommend-item" v-for="recommend in rating.recommend">{{recommend}}</span>
            </div>
          </div>
         
        </li>
      </ul>
    </div>
  </cube-scroll>
</template>

<script>
  import Star from 'components/star/star'
  import Split from 'components/split/split'
  import RatingSelect from 'components/rating-select/rating-select'
  import { getRatings } from 'api'
  import moment from 'moment'
  import ratingMixin from 'common/mixins/rating'

  const ALL = 2

  export default {
  	name: 'ratings',
    mixins: [ratingMixin],
  	props: {
  	  data: {
  	  	type: Object,
        default() {
          return {}
        }
  	  }
  	},
    data() {
      return {
        ratings: [],
        scrollOptions: {
          click: false,
          directionLockThreshold: 0
        }
      }
    },
    computed: {
      seller() {
        return this.data.seller || {}
      }
    },
    methods: {
      fetch() {
        if (!this.fetched) {
          this.fetched = true
          getRatings({
            id: this.seller.id
          }).then((ratings) => {
            this.ratings = ratings
          })
        }     
      },
      format(time) {
        return moment(time).format('YYYY-MM-DD hh:mm')
      }
    },
    components: {
      Star,
      Split,
      RatingSelect
    }
  }
</script>

<style lang="stylus" scoped>
  .ratings
    white-space: normal
    bottom: 48px
    .overall
      display: flex
      padding: 18px 0
      .text
        line-height: 18px
        font-size: 12px
        color: rgb(7, 17, 27)
      .overall-left
        padding: 0 24px
        text-align: center
        border-right: 1px solid rgba(7, 17, 27, .1)
        .score
          margin-bottom: 6px
          font-size: 24px
          color: rgb(255, 153, 0)
        .rank
          margin-top: 8px
          font-size: 10px
          color: rgb(147, 153, 159)
      .overall-right
        padding-left: 24px
        .right-item
          display: flex
          align-items: center
          margin-bottom: 8px
          .text
            margin-right: 12px
            line-height: 18px
          .star-wrapper
            margin-right: 12px
          .right-score
            font-size: 12px
            color: rgb(255, 153, 0)
          &:last-child
            margin-bottom: 0
          .time
            font-size: 12px
            color: rgb(147, 153, 159)
    .rating-select-wrapper
      padding: 0 18px
      border-bottom: 1px solid rgba(7, 17, 27, .1) 
    .ratings-list
      margin: 0 18px
      padding: 18px 0
      border-bottom: 1px solid rgba(7, 17, 27, .1)
      &:last-child
        border-bottom: none
      .rating-user
        display: flex
        font-size: 10px
        .avatar
          flex: 0 0 28px
          width: 28px
          height: 28px
          margin-right: 12px
          img
            border-radius: 50%
        .info
          flex: 1
          .name
            margin-bottom: 4px
            line-height: 12px
            color: rgb(7, 17, 27)
          .score-time
            margin-bottom: 6px
            .star-wrapper
              display: inline-block
              margin-right: 6px
            .delivery-time
              color: rgb(147, 153, 159)
        .rate-time
          line-height: 12px
          font-weight: 200
          color: rgb(147, 153, 159)
      .content
        padding: 0 18px 0 40px
        .text
          line-height: 16px
          font-size: 12px
          color: rgb(7, 17, 27)
        .recommend
          margin-top: 8px
          .icon-thumb
            margin-right: 8px
            font-size: 12px
            &.icon-thumb_up
              color: rgb(0, 160, 220)
            &.icon-thumb_down
              color: rgb(183, 187, 191)
          .recommend-item
            display: inline-block
            height: 16px
            margin-right: 8px
            margin-bottom: 8px
            padding: 0 6px
            line-height: 16px
            font-size: 9px
            color: rgb(147, 153, 159)
            border: 1px solid rgba(7, 17, 27, .1)
            border-radius: 1px
</style> 
