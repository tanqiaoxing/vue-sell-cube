<template>
  <transition name="fade">
    <div v-show="visible" class="header-detail clearfix" @touchmove.stop.prevent>
  	  <div class="detail-wrapper">
  	    <div class="detail-main">
  	      <h1 class="name">{{seller.name}}</h1>
  	      <div class="star-wrapper">
  	        <star :size="48" :score="seller.score"></star>
  	      </div>
  	      <div class="title" v-show="seller.supports">
  	        <div class="line"></div>
  	        <div class="text">优惠信息</div>
  	        <div class="line"></div>
  	      </div>
  	      <div class="support" v-show="seller.supports">
            <ul>
              <li v-for="support in seller.supports" class="support-li">
                <support-ico :size=2 :type="support.type"></support-ico>
                <span class="support-desc">{{support.description}}</span>
              </li>
            </ul>
  	      </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="bulletin-text">{{seller.bulletin}}</p>
          </div>
  	    </div>  		
  	  </div>
  	  <div class="detail-close" @click="hide">
	      <i class="icon-close"></i>
	    </div>
    </div>
  </transition>
</template>

<script>
  import SupportIco from 'components/support-ico/support-ico'
  import star from 'components/star/star'
  import popupMixin from 'common/mixins/popup'

  export default {
    mixins: [popupMixin],
    name: 'header-detail',
    props: {
      seller: {
		    type: Object,
		    default() {
		      return {}
		    }
	    }
    },
    components: {
      SupportIco,
      star
    }
  }
</script>

<style lang="stylus" scoped>
  .header-detail
    position: fixed
    z-index: 100
    top: 0
    left: 0
    width: 100%
    height: 100%
    overflow: auto
    color: #fff
    opacity:1
    background: rgba(7, 17, 27, .8)
    &.fade-enter-active, &.fade-leave-active
      transition: all .5s
    &.fade-enter, &.fade-leave-to
      opacity: 0
      background: rgba(7, 17, 27, 0)
    .detail-wrapper
      display: inline-block
      width: 100%
      min-height: 100%
      .detail-main
        margin-top: 64px
        padding-bottom: 64px
        .name
          margin-bottom: 16px
          line-height: 16px
          text-align: center
          font-size: 16px
          font-weight: 700
        .star-wrapper
          text-align: center
        .title
          display: flex
          width: 80%
          margin: 28px auto 24px auto
          .text
            padding: 0 12px
            line-height: 14px
            font-size: 14px
            font-weight: 700
          .line
            flex: 1
            position: relative
            top: -6px
            border-bottom: 1px solid rgba(255, 255, 255, .2)
        .support
          width: 80%
          margin: 0 auto
          .support-li
            display: flex
            margin: 0 0 12px 12px
            &:last-child
              margin-bottom: 0
            .support-desc
              margin-left: 6px
              line-height: 16px
              font-size: 12px
              font-weight: 200
        .bulletin
          width: 80%
          margin: 0 auto
          .bulletin-text
            line-height: 24px
            font-size: 12px
            font-weight: 200
    .detail-close
      position: relative
      width: 32px
      height: 32px
      margin: -64px auto 0 auto
      font-size: 32px
      clear: both
      color: rgba(255, 255, 255, .5)
</style>
