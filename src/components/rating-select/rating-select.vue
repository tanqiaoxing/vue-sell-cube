<template>
  <div class="rating-select">
    <div class="select">
  	  <div @click="select(2)" class="rating-type blue" :class="{'active':selectType===2}">
  	    <span class="desc">{{desc.all}}</span>
  	    <span class="num">{{all}}</span>
  	  </div>
  	  <div @click="select(0)" class="rating-type blue" :class="{'active':selectType===0}">
  	    <span class="desc">{{desc.positive}}</span>
  	    <span class="num">{{positives.length}}</span>
  	  </div>
  	  <div @click="select(1)" class="rating-type gray" :class="{'active':selectType===1}">
        <span class="desc">{{desc.negative}}</span>
  	    <span class="num">{{negatives.length}}</span>
      </div>
    </div>
    <div class="switch">
      <span @click="toggle" class="icon-check_circle" :class="{'highlight':onlyContent}"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script>
  const ALL = 2
  const POSITIVE = 0
  const NEGATIVVE = 1

  const EVENT_SELECT = 'select'
  const EVENT_TOGGLE = 'toggle'

  export default {
  	name: 'rating-select',
  	props: {
  	  ratings: {
  	  	type: Array,
  	  	default() {
  	  	  return []
  	  	}
  	  },
      desc: {
      	type: Object,
      	default() {
      	  return {
            all: '全部',
      	    positive: '满意',
      	    negative: '不满意'
      	  }
      	}
      },
      selectType: {
      	type: Number,
      	default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      }
  	},
  	computed: {
  	  all() {
        return this.ratings.length
      },
      positives() {
      	return this.ratings.filter((rating) => {
      	  return rating.rateType === POSITIVE
      	})
      },
      negatives() {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVVE
        })
      }
  	},
  	methods: {
  	  select(type) {
  	  	this.$emit(EVENT_SELECT, type)
  	  },
      toggle() {
        this.$emit(EVENT_TOGGLE)
      }
  	}
  }
</script>

<style lang="stylus" scoped>
  .rating-select
    .select
      padding: 18px 0
      border-bottom: 1px solid rgba(7, 17, 27, .1)
      .rating-type
        display: inline-block
        padding: 18px
        margin-right: 8px
        border-radius: 2px
        .desc
          margin-right: 4px
          font-size: 12px
        .num
          font-size: 8px
      .blue
        background: rgba(0, 160, 220, .2)
        &.active
          color: #fff
          background: rgb(0, 160, 220)
      .gray
        background: rgba(77, 85, 93, .2)
        &.active
          color: #fff       
          background: rgb(77, 85, 93)
    .switch
      padding: 12px 0
      color: rgb(147, 153, 159)
      .icon-check_circle
        display: inline-block
        vertical-align: middle
        margin-right: 4px
        font-size: 24px        
        &.highlight
          color: #00b43c
      .text
        display: inline-block
        vertical-align: middle
        font-size: 12px
        line-height: 24px
</style>

