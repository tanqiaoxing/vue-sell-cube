<template>
  <cube-scroll
    class="seller"
    ref="sellerScroll"
    :options="sellerScrollOptions"
  >
    <div class="info">
      <div class="info-top">
        <div class="info-left">
          <p class="title">{{seller.name}}</p>
          <div class="rating">
            <div class="star-wrapper">
              <star :size=36 :score="seller.score"></star>
            </div>
            <div class="rating-count">({{seller.ratingCount}})</div>
            <div class="sell-count">月售{{seller.sellCount}}单</div>
          </div>
        </div>
        <div @click="toggleFavorite" class="favorite">
          <span class="icon-favorite" :class="{'active':favorite}"></span>
          <div class="favorite-text">{{favoriteText}}</div>
        </div>
      </div>
      <div class="info-bottom">
        <div class="info-item border-right">
          <span class="text">起送价</span>
          <div class="number">
            <span class="num">{{seller.minPrice}}</span>
            <span class="piece">元</span>
          </div>
        </div>
        <div class="info-item border-right">
          <span class="text">商家配送</span>
          <div class="number">
            <span class="num">{{seller.deliveryPrice}}</span>
            <span class="piece">元</span>
          </div>
        </div>
        <div class="info-item">
          <span class="text">平均配送时间</span>
          <div class="number">
            <span class="num">{{seller.deliveryTime}}</span>
            <span class="piece">分钟</span>
          </div>
        </div>
      </div>
    </div>
    <split></split>
    <div class="bulletin">
      <div class="title">公告与活动</div>
      <p class="content">{{seller.bulletin}}</p>
    </div>
    <ul>
      <li class="support" v-for="(support, index) in seller.supports" :key="index">
        <div class="support-ico-wrapper">
          <support-ico :size=4 :type="support.type"></support-ico>
        </div>
        <div class="description">{{support.description}}</div>
      </li>
    </ul>
    <split></split>
    <div class="picture">
      <div class="title">商家实景</div>
      <cube-scroll
        :options="picScrollOptions"
        class="pic-wrapper"
      >
        <ul>
          <li class="pic-item" v-for="(picture, index) in seller.pics" :key="index">
            <img width=120 height=90 :src="picture" />
          </li>
        </ul>
      </cube-scroll>
    </div>
    <split></split>
    <div class="infos">
      <div class="title">商家信息</div>
      <ul>
        <li class="information" v-for="(info, index) in seller.infos" :key="index">
          {{info}}
        </li>
      </ul>
    </div>
  </cube-scroll>
</template>

<script>
  import Star from 'components/star/star'
  import Split from 'components/split/split'
  import SupportIco from 'components/support-ico/support-ico'
  import { saveToLocal, loadFromLocal } from 'common/js/storage'

  const KEY_FAV = 'favorite'

  export default {
    name: 'seller',
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
        favorite: false,
        sellerScrollOptions: {
          click: false,
          directionLockThreshold: 0
        },
        picScrollOptions: {
          scrollX: true,
          stopPropagation: true,
          directionLockThreshold: 0
        }
      }
    },
    created() {
      this.favorite = loadFromLocal(this.seller.id, KEY_FAV, false)
    },
    computed: {
      seller() {
        return this.data.seller || {}
      },
      favoriteText() {
        return this.favorite ? '已收藏' : '收藏'
      }
    },
    methods: {
      toggleFavorite() {
        this.favorite = !this.favorite
        saveToLocal(this.seller.id, KEY_FAV, this.favorite)
      }
    },
    components: {
      Star,
      Split,
      SupportIco
    }
  }
</script>

<style lang="stylus" scoped>
  .seller
    .title
      line-height: 14px
      font-size: 14px
      color: rgb(7, 17, 27)
    .info
      position: relative
      padding: 18px
      .info-top
        padding-bottom: 18px
        border-bottom: 1px solid rgba(7, 17, 27, .1)
        .rating
          display: flex
          align-items: center
          margin-top: 8px
          color: rgb(77, 85, 93)
          .star-wrapper
            margin-right: 8px
            vertical-align: middle
          .rating-count
            margin-right: 12px
            font-size: 10px
          .sell-count
            font-size: 10px
        .favorite
          position: absolute
          top: 18px
          right: 18px
          width: 36px
          text-align: center
          .icon-favorite
            font-size: 24px
            color: #ccc
            &.active
              color: rgb(240, 20, 20)
          .favorite-text
            margin-top: 4px
            font-size: 10px
            color: rgb(77, 85, 93)
      .info-bottom
        display: flex
        padding-top: 18px
        .info-item
          flex: 1
          display: inline-block
          text-align: center
          &.border-right
            border-right: 1px solid rgba(7, 17, 27, .1)
          .text
            font-size: 10px
            color: rgb(147, 153, 159)
          .number
            margin-top: 4px
            .num
              font-size: 24px
              font-weight: 200
              color: rgb(7, 17, 27)
            .piece
              font-size: 10px
              color: rgb(147, 153, 159)
    .bulletin
      padding: 18px
      .content
        padding: 8px 12px 16px
        line-height: 24px
        font-size: 12px
        font-weight: 200
        color: rgb(240, 20, 20)
        white-space: normal
    .support
      display: flex
      align-items: center
      padding: 16px 0
      margin: 0 16px
      border-top: 1px solid rgba(7, 17, 27, .1)
      .support-ico-wrapper
        margin-left: 12px
      .description
        margin-left: 6px
        line-height: 16px
        font-size: 12px
        font-weight: 200
        color: rgb(7, 17, 27)
    .picture
      padding: 18px 0 18px 18px
      .pic-wrapper
        display: flex
        .pic-item
          display: inline-block
          width: 120px
          height: 90px
          margin-top: 12px
          margin-right: 6px
    .infos
      padding: 18px
      .title
        margin-bottom: 12px
      .information
        padding: 16px 12px
        border-top: 1px solid rgba(7, 17, 27, .1)
        line-height: 16px
        font-size: 12px
        font-weight: 200
        color: rgb(7, 17, 27)
        white-space: normal
</style>
