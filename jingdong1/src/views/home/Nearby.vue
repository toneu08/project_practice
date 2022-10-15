<template>
  <div class = "nearby">
    <div class ="nearby__title">附近店铺</div>
    <router-link :to = "`/shop/${ item._id }`"
      v-for = "item in nearbyList"
      :key = "item._id">
      <ShopInfo :item = "item"/>
    </router-link>
  </div>
</template>

<script>
import { ref } from 'vue'
import { get } from '../../utils/request'
import ShopInfo from '../../components/ShopInfo'

// 函数封装，通过return导出
const useNearbyListEffect = () => {
  const nearbyList = ref([])
  const getNearbyList = async () => {
    const result = await get('/api/shop/hot-list')
    if (result?.errno === 0 && result?.data?.length) {
      nearbyList.value = result.data
    }
  }
  return { nearbyList, getNearbyList }
}
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Nearby',
  components: { ShopInfo },
  setup () {
    const { nearbyList, getNearbyList } = useNearbyListEffect()
    getNearbyList()
    return { nearbyList }
    // [{ id: 1,
    //   imgUrl: 'http://www.dell-lee.com/imgs/vue3/near.png',
    //   title: '沃尔玛',
    //   tags: ['月售1万+', '起送¥0', '基础运费¥5'],
    //   desc: 'VIP尊享满89元减4元运费券（每月3张）'
    // }]
  }
}
</script>

<style lang = "scss" scoped>
  @import '../../style/viriables.scss';
  .nearby {
    &__title {
      margin: .16rem 0 .02rem 0;
      font-size: .18rem;
      color: $content-fontcolor;
    }
    a {
      text-decoration: none;
    }
  }
</style>
