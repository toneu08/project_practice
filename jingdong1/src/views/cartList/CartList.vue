<template>
  <div class = "wrapper">
    <div class = "title">我的全部购物车()</div>
    <div class = "products">
    <div class = "products__title">
      店铺名
      <!-- shopName一直是undefined，然后值传不进来，目前无法修改 -->
    </div>
    <div class = "products__wrapper">
      <div class = "products__list">
        <div
          class = "products__item"
          v-for="item in productList"
          :key = "item._id"
        >
          <img class = "products__item__img" :src="item.imgUrl"/>
          <div class = "products__item__detail">
            <h4 class = "products__item__title">2</h4>
            <p class = "products__item__price">
              <span>
                <span class = "products__item__yen">&yen;</span>

              </span>
              <span class = "products__item__total">
                <span class = "products__item__yen">&yen;</span>

              </span>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
  </div>
  <Docker :currentIndex = "1"/>
</template>

<script>
import { useRoute } from 'vue-router'
import { useCommonCartEffect } from '../../effects/cartEffects'
import Docker from '../../components/Docker'
export default {
  name: 'CartList',
  components: { Docker },
  setup () {
    const route = useRoute()
    const shopId = route.params.id
    const { productList } = useCommonCartEffect(shopId)
    return { productList }
  }
}
</script>

<style lang = "scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';
.wrapper {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  margin-bottom: .5rem;
  overflow-y: scroll;
  background: rgb(248, 248, 248);
}
.title {
  position: relative;
  font-size: .16rem;
  color: $content-fontcolor;
  background: $bgColor;
  line-height: .44rem;
  text-align: center;
  margin-bottom: .16rem;
}
.products {
  margin: .16rem .18rem .16rem .18rem;
  background: $bgColor;
  &__title {
    padding: .16rem;
    font-size: .16rem;
    color: $content-fontcolor;
  }
  &__wrapper {
    overflow-y: scroll;
    left: 0;
    bottom: .6rem;
    right: 0;
    top: 2.6rem;
  }
  &__list {
    background: $bgColor;
  }
  &__item {
    position: relative;
    display: flex;
    padding: 0 .16rem 0.16rem .16rem;
    &__img {
      width: .46rem;
      height: .46rem;
      margin-right: .16rem;
    }
    &__detail {
      flex: 1;
    }
    &__title {
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $content-fontcolor;
      @include ellipsis;
    }
    &__price {
      display: flex;
      margin: .06rem 0 0 0;
      line-height: .2rem;
      font-size: .14rem;
      color: $hightlight-fontColor;
    }
    &__total {
      flex: 1;
      text-align: right;
      color: $dark-fontColor;
    }
    &__yen {
      font-size: .13rem;
    }
  }
}

</style>
