<template>
  <div>
    <detail-nav-bar></detail-nav-bar>
    <detail-swiper :top-images="topImages"></detail-swiper>
    <detail-base-info :data="goods"></detail-base-info>
  </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar'
import DetailSwiper from './childComps/DetailSwiper'
import DetailBaseInfo from './childComps/DetailBaseInfo'

import {getDetail, GoodsInfo} from 'network/detail'



export default {
  // 组件名称
  name: 'Detail',
  components: {
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo
  },
  data() {
    return {
      iid: null,
      topImages: [],
      goods: {}
    }
  },
  created() {
    // 保存传入的iid
    this.iid = this.$route.params.iid
    // console.log(this.$route.params)

    // 根据iid请求详情数据
    getDetail(this.iid).then(res => {
      // console.log(res)
      let data = res.result
      console.log(data)
      // 1.获取顶部的轮播图数据
      this.topImages = data.itemInfo.topImages
       // 2.获取商品信息
      this.goods = new GoodsInfo(data.itemInfo, data.columns, data.shopInfo.services);
      
    })
  }
}
</script> 

<style scoped>

</style>