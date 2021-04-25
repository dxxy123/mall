<template>
  <div class="detail-swiper">
    <swiper class="dswiper-item">
      <swiper-item v-for="(item, index) in topImages" :key="index">
        <img :src="item">
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
import {Swiper, SwiperItem} from 'components/common/swiper'

export default {
  // 组件名称
  name: 'DetailSwiper',
  components: {
    Swiper,
    SwiperItem,
  },
  props: {
    topImages: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script> 

<style scoped>
  .dswiper-item {
    height: 300px;
    overflow: hidden;
  }
</style>