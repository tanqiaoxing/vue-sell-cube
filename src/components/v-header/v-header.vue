<template>
  <div class="header" @click="showDetail">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar" />
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟
        </div>
        <div v-if="seller.supports" class="support">
          <support-ico :size=1 :type="seller.supports[0].type"></support-ico>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img width="100%" height="100%" :src="seller.avatar" />
    </div>
  </div>
</template>

<script>
  import SupportIco from 'components/support-ico/support-ico'

  export default {
    name: 'v-header',
    props: {
      seller: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    methods: {
      showDetail() {
        this.headerDetailComp = this.headerDetailComp || this.$createHeaderDetail({
            $props: {
              seller: 'seller'
            }
        })
        this.headerDetailComp.show()
      }
    },
    components: {
      SupportIco
    }
  }
</script>

<style lang="stylus" scoped>
  @import "~common/stylus/mixin"

  .header
    position: relative
    color: #fff
    background: rgba(7, 17, 27, 0.5)
    overflow: hidden
    .content-wrapper
      position: relative
      display: flex
      padding: 24px 12px 18px 24px
      .avatar
        flex: 0 0 64px
        margin-right: 16px
        img
          border-radius: 2px
      .content
        flex: 1
        .title
          display: flex
          margin-bottom: 8px
          .brand
            width: 30px
            height: 18px
            margin-right: 6px
            vertical-align: top
            bg-image('brand')
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            font-size: 16px
            font-weight: 700
            line-height: 18px
        .description
          margin-bottom: 10px
          line-height: 12px
          font-size: 12px
          font-weight: 200
        .support
          display: flex
          .text
            margin-left: 4px
            line-height: 12px
            font-size: 10px
            font-weight: 200
      .support-count
        position: absolute
        top: 64px
        right: 12px
        display: flex
        height: 24px
        padding: 0 8px
        line-height: 24px
        font-size: 10px
        font-weight: 200
        background: rgba(0,0,0,0.2)
        border-radius: 14px
        .icon-keyboard_arrow_right
          margin-left: 2px
          line-height: 24px
    .bulletin-wrapper
      display: flex
      align-items: center
      height: 28px
      padding: 0 12px
      background: rgba(0, 0, 0, 0.2)
      .bulletin-title
        flex: 0 0 22px
        width: 22px
        height: 12px
        margin-right: 4px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        flex: 1
        overflow: hidden
        white-space: nowrap
        text-overflow: ellipsis
        font-size: 10px
        font-weight: 200
      .icon-keyboard_arrow_right
        flex: 0 0 10px
    .background
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
</style>
