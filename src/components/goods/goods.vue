<template>
  <div class="goods">
    <div class="scroll-nav-wrapper">
      <cube-scroll-nav
        :side=true
        :data="goods"
        :options="scrollOptions"
        v-if="goods.length"
      >
        <template slot="bar" slot-scope="props">
          <cube-scroll-nav-bar
            direction="vertical"
            :labels="props.labels"
            :txts="barTxts"
            :current="props.current"
          >
            <template slot-scope="props">
              <div class="text">
                <support-ico 
                  v-if="props.txt.type>=1"
                  :size=3
                  :type="props.txt.type"
                ></support-ico>
                <span>{{props.txt.name}}</span>
                <span class="num" v-if="props.txt.count">
                  <bubble :num="props.txt.count" :type=2></bubble>
                </span>    
              </div>
            </template>
          </cube-scroll-nav-bar>
        </template>
        <cube-scroll-nav-panel
          v-for="good in goods"
          :label="good.name"
          :title="good.name"
          :key="good.name"
        >
          <ul>
            <li 
              class="food-item" 
              v-for="food in good.foods" 
              @click="selectFood(food)"
              :key="food.name"
            >
              <div class="avatar">
                <img width="57" height="57" :src="food.icon" />
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>                 
                </div>
                <div class="price">
                  <span class="now-price-symbol">¥</span>
                  <span class="now-price">{{food.price}}</span>
                  <span v-show="food.oldPrice" class="old-price">¥{{food.oldPrice}}</span>
                </div>             
              </div>
              <div class="cart-control-wrapper">
                <cart-control @add="onAdd" :food="food"></cart-control>
              </div>
            </li>
          </ul>
        </cube-scroll-nav-panel>
      </cube-scroll-nav>
    </div>
    <div class="shop-cart-wrapper">
      <shop-cart 
        ref="shopCart"
        :select-foods="selectFoods" 
        :delivery-price="seller.deliveryPrice" 
        :min-price="seller.minPrice"
      >
      </shop-cart>
    </div>    
  </div>
</template>

<script>
  import { getGoods } from 'api'
  import ShopCart from 'components/shop-cart/shop-cart'
  import CartControl from 'components/cart-control/cart-control'
  import SupportIco from 'components/support-ico/support-ico'
  import Bubble from 'components/bubble/bubble'

  export default {
  	name: 'goods',
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
        goods: [],
        selectedFood: {},
        scrollOptions: {
          click: false,
          directionLockThreshold: 0
        }
      }
    },
    computed: {
      seller() {
        return this.data.seller
      },
      selectFoods() {
        let ret = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              ret.push(food)
            }          
          })
        })
        return ret
      },
      barTxts() {
        let ret = []
        this.goods.forEach((good) => {
          const {type, name, foods} = good
          let count = 0
          foods.forEach((food) => {
            count += food.count || 0
          })
          ret.push({
            type,
            name,
            count
          })
        })
        return ret
      }
    },
    methods: {
      fetch() {
        if (!this.fetched) {
          this.fetched = true
          getGoods({
            id: this.seller.id
          }).then((goods) => {
            this.goods = goods
          })
        }     
      },
      onAdd(el) {
        this.$refs.shopCart.drop(el)
      },
      selectFood(food) {
        this.selectedFood = food
        this._showFood()
        this._showShopCartSticky()
      },
      _showFood() {
        this.foodComp = this.foodComp || this.$createFood({
          $props: {
            food: 'selectedFood'
          },
          $events: {
            leave: () => {
              this._hideShopCartSticky()
            },
            add: (el) => {
              this.shopCartStickyComp.drop(el)
            }
          }
        })
        this.foodComp.show()
      },
      _showShopCartSticky() {
        this.shopCartStickyComp = this.shopCartStickyComp || this.$createShopCartSticky({
          $props: {
            selectFoods: 'selectFoods',
            deliveryPrice: this.seller.deliveryPrice,
            minPrice: this.seller.minPrice,
            fold: true
          }
        })
        this.shopCartStickyComp.show()
      },
      _hideShopCartSticky() {
        this.shopCartStickyComp.hide()
      }
    },
    components: {
      ShopCart,
      CartControl,
      SupportIco,
      Bubble
    }
  }
</script>

<style lang="stylus" scoped>
  .goods
    position: relative
    height: 100%
    .scroll-nav-wrapper
      position: absolute
      width: 100%
      top: 0
      left: 0
      bottom: 48px
    >>> .cube-scroll-nav-bar
      width: 80px
      white-space: normal
      overflow: hidden
    >>> .cube-scroll-nav-bar-item
      display: flex
      align-items: center
      padding: 0 10px
      height: 54px
      line-height: 14px
      font-size: 12px
      font-weight: 200
      color: rgb(7, 17, 27)
      background: #f3f5f7
      .text
        flex: 1
        position: relative
        .num
          position: absolute
          right: -8px
          top: -10px
        .support-ico
          display: inline-block
          vertical-align: top
          margin-right: 4px  
    >>> .cube-scroll-nav-bar-item_active
      color: #fc9153
      background: #fff
    >>> .cube-scroll-nav-panel-title
      height: 26px
      padding-left: 14px
      line-height: 26px
      font-size: 12px
      color: rgb(147, 153, 159)
      background: #f3f5f7
    .food-item
      position: relative
      display: flex
      margin: 0 18px
      padding: 18px 0
      border-bottom: 1px solid rgba(7, 17, 27, .1)
      &:last-child
        border: none
        margin-bottom: 0
      .avatar
        flex: 0 0 57px
        margin-right: 10px
      .content
        .name
          margin-top: 2px
          line-height: 14px
          font-size: 14px
          color: rgb(7, 17, 27)
          font-weight: 700
        .desc, .extra
          margin-top: 8px
          line-height: 10px
          font-size: 10px
          color: rgb(147, 153, 159)
          font-weight: 700
        .extra
          .count
            margin-right: 12px
        .price
          .now-price-symbol
            font-size: 10px
            color: rgb(240, 20, 20)
          .now-price
            line-height: 24px
            font-size: 14px
            font-weight: 700
            color: rgb(240, 20, 20)
          .old-price
            text-decoration: underline-through
            line-height: 24px
            font-size: 10px
            font-weight: 700
            color: rgb(147, 153, 159)
      .cart-control-wrapper
        position: absolute
        bottom: 12px
        right: 0
    .shop-cart-wrapper
      position: absolute
      left: 0
      bottom: 0
      width: 100%
      z-index: 150
</style>

