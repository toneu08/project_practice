<template>
  <div class = "wrapper">
    <StaticPart />
    <Nearby />
  </div>
  <Docker :currentIndex = "0"/>
</template>

<script>
import StaticPart from './StaticPart'
import Nearby from './Nearby'
import Docker from '../../components/Docker'
export default {
  name: 'Home',
  components: { StaticPart, Nearby, Docker }
}
</script>

<style lang = "scss" scoped>
  @import '../../style/viriables.scss';
  @import '../../style/mixins.scss';
  .wrapper {
    overflow-y: auto;
    position: absolute;
    left: 0;
    top: 0;
    bottom: .5rem;
    right: 0;
    padding: 0 .18rem .1rem .18rem;;
  }
</style>
