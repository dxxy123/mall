<template>
  <swiper>
    <swiper-item v-for="(item, index) in banners" :key="index">
      <a :href="item.link">
        <!-- @load="imageLoad" -->
        <img :src="item.image" alt="" >
      </a>
    </swiper-item>
  </swiper>

</template>

<script>
import {Swiper, SwiperItem} from 'components/common/swiper'

export default {
  // 组件名称
  name: 'HomeSwiper',
  props: {
    banners: {
      type: Array,
      default() {
        return []
      }
    }
  },
  data() {
    return {
      // isLoad: fslae
    }
  },
  components: {
    Swiper,
    SwiperItem
  },
  // methods: {
  //   // 发送一次轮播图的一张图片加载完成事件
  //   imageLoad() {
  //     if(!isLoad) {
  //       this.$emit('swiperImageLoad')
  //       this.isLoad = true
  //     }
  //   }
  // }
}
</script> 

<style scoped>

</style>