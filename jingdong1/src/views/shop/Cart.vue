<template>
  <div class = "mask"
    v-if = "showCart && calculations.total >0 "
    @click="handleCartShowChange"
  />
  <div class = "cart">
    <div class = "product" v-if = "showCart && calculations.total >0 ">
      <div class = "product__header">
        <div
        class = "product__header__all"
        @click = "() => setCartItemsChecked(shopId)"
        >
          <span
          class = "product__header__icon iconfont"
          v-html = "calculations.allChecked ? '&#xe618;' : '&#xe66c;'"
          >
          </span>
          全选
        </div>
        <div
        class = "product__header__clear"
        >
          <span class = "product__header__clear__btn"
          @click = "cleanCartProducts(shopId)">清空购物车</span>
        </div>
      </div>

      <div
        v-for = "item in productList"
        :key = "item._id"
        class="product__item"
      >
        <div
          class = "product__item__checked iconfont"
          v-html = "item.check ? '&#xe618;' : '&#xe66c;'"
          @click = "() => changeCartItemChecked(shopId, item._id)"
        />
        <img class = "product__item__img" :src="item.imgUrl" />
        <div class = "product__item__detail">
          <h4 class = "product__item__title">{{item.name}}</h4>
          <p class = "product__item__price">
            <span class = "product__item__yen">&yen;</span>{{item.price}}
            <span class = "product__item__origin">&yen;{{item.oldPrice}}</span>
          </p>
        </div>
        <div class = "product__number">
          <span class = "product__number__minus"
          @click = "() => { changeCartItemInfo( shopId, item._id, item, -1) }"
          >-</span>
          <span class = "product__number__add">{{ item.count || 0 }}</span>
          <span class = "product__number__plus"
          @click = "() => { changeCartItemInfo( shopId, item._id, item, 1) }"
          >+</span>
        </div>
      </div>
    </div>
    <div class = "check">
      <div class = "check__icon">
        <img
          src = "http://www.dell-lee.com/imgs/vue3/basket.png"
          class = "check__icon__img"
          @click="handleCartShowChange"
        />
        <div class = "check__icon__tag">{{calculations.total}}</div>
      </div>
      <div class = " check__info ">
        总计：<span class = " check__info__price ">&yen;{{calculations.price}}</span>
      </div>
      <div class = " check__btn" v-show="calculations.total > 0">
        <router-link :to = "{ path: `/OrderConfirmation/${shopId}` }">
          去结算
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import { useStore } from 'vuex'
import { useRoute } from 'vue-router'
import { useCommonCartEffect } from '../../effects/cartEffects'

// 获取购物车信息逻辑
const useCartEffect = (shopId) => {
  const store = useStore()
  const { productList, calculations, changeCartItemInfo } = useCommonCartEffect(shopId)

  const changeCartItemChecked = (shopId, productId) => {
    store.commit('changeCartItemChecked', { shopId, productId })
  }
  const cleanCartProducts = (shopId) => {
    store.commit('cleanCartProducts', { shopId })
  }
  const setCartItemsChecked = (shopId) => {
    store.commit('setCartItemsChecked', { shopId })
  }
  return {
    calculations,
    productList,
    changeCartItemInfo,
    changeCartItemChecked,
    cleanCartProducts,
    setCartItemsChecked
  }
}

// 展示隐藏购物车逻辑
const toggleCartEffect = () => {
  const showCart = ref(false)
  const handleCartShowChange = () => {
    showCart.value = !showCart.value
  }
  return { showCart, handleCartShowChange }
}

export default {
  name: 'Cart',
  setup () {
    const route = useRoute()
    const shopId = route.params.id
    const {
      calculations, productList, setCartItemsChecked,
      changeCartItemInfo, changeCartItemChecked, cleanCartProducts
    } = useCartEffect(shopId)
    const { showCart, handleCartShowChange } = toggleCartEffect()

    return {
      calculations,
      productList,
      shopId,
      showCart,
      changeCartItemInfo,
      changeCartItemChecked,
      cleanCartProducts,
      setCartItemsChecked,
      handleCartShowChange
    }
  }
}
</script>

<style lang = "scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';

.mask {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  background-color: rgba(0,0,0, .5);
  z-index: 1;
}
.cart {
  z-index: 2;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: $bgColor;
  border-radius: .15rem .15rem 0 0;
}

.product {
  overflow-y: scroll;
  flex: 1;
  &__header {
    display: flex;
    line-height: .5rem;
    border-bottom: .01rem solid #f1f1f1;
    font-size: .14rem;
    color: $content-fontcolor;
    &__all {
      width: .64rem;
      margin-left: .16rem;
    }
    &__icon {
      color: $btn-bgColor;
      font-size: .2rem;
      line-height: .52rem;
      margin-right: .08rem;
    }
    &__clear {
      flex: 1;
      margin-right: .16rem;
      text-align: right;
      &__btn {
        line-height: .54rem;
        display: inline-block;
      }
    }
  }
  &__item {
    position: relative;
    display: flex;
    margin: .16rem .16rem;
    &__checked {
      color: $btn-bgColor;
      font-size: .2rem;
      width: .19rem;
      height:.19rem;
      line-height: .46rem;
      margin-right: .16rem;
    }
    &__detail {
      overflow: hidden;
    }
    &__img {
      width: .46rem;
      height: .46rem;
      margin-right: .16rem;
    }
    &__title {
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $content-fontcolor;
      @include ellipsis;
    }
    &__price {
      margin: .06rem 0 0 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $hightlight-fontColor;
    }
    &__yen {
      font-size: .12rem;
    }
    &__origin {
      margin-left: .06rem;
      line-height: .2rem;
      font-size: .12rem;
      color: $light-fontColor;
      text-decoration: line-through;
    }
    .product__number {
      position: absolute;
      display: inline-block;
      right: 0rem;
      bottom: .12rem;
      line-height: .2rem;
      // margin-bottom: .02rem;
      &__add,&__minus,&__plus{
        display: inline-block;
        width: .17rem;
        height: .17rem;
        text-align: center;
      }
      &__add{
        font-size: .14rem;
      }
      &__minus,&__plus {
        font-size: .2rem;
        line-height: .16rem;
        border-radius: 50%;
        border: .01rem solid $medium-fontColor;
      }
      &__minus{
        border: .01rem solid $medium-fontColor;
        color: $medium-fontColor;
        margin-right: .05rem;
      }
      &__plus {
        background: $btn-bgColor;
        color:$bgColor;
        margin-left: .05rem;
      }
    }
  }
}

.check {
  display: flex;
  box-sizing: border-box;
  line-height: .49rem;
  height:.49rem;
  border-top: .01rem solid $content-bgColor;
  &__icon {
    position: relative;
    width: .84rem;
    &__img {
      display: block;
      margin: .12rem auto;
      width: .28rem;
      height: .26rem;
    }
    &__tag {
      position: absolute;
      right: .14rem;
      top: .02rem;
      width: .3rem;
      height: .3rem;
      line-height: .32rem;
      background-color: $hightlight-fontColor;
      border-radius: 50%;
      font-size: .16rem;
      text-align: center;
      color: #fff;
      transform: scale(.5);
    }
  }
  &__info {
    flex: 1;
    color: $content-fontcolor;
    font-size: .12rem;
    &__price {
      font-size: .18rem;
      color: $hightlight-fontColor;
    }
  }
  &__btn {
    width: .98rem;
    font-size: .14rem;
    text-align: center;
    background-color: #4FB0F9;
    a {
      color: $bgColor;
      text-decoration: none;
    }
  }
}
</style>
