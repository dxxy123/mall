<template>
  <tab-bar>
    <!-- 默认颜色为空 -->
    <tab-bar-item path='/home' activeColor="">
      <img slot="item-icon" src="~assets/img/tabbar/home.svg">
      <img slot="item-icon-active" src="~assets/img/tabbar/home_active.svg">
      <div slot="item-text">首页</div>
    </tab-bar-item>
    <tab-bar-item path='/category' activeColor="deeppink">
      <img slot="item-icon" src="~assets/img/tabbar/category.svg">
      <img slot="item-icon-active" src="~assets/img/tabbar/category_active.svg">
      <div slot="item-text">分类</div>
    </tab-bar-item>
    <tab-bar-item path='/cart' activeColor="deeppink">
      <img slot="item-icon" src="~assets/img/tabbar/shopcart.svg">
      <img slot="item-icon-active" src="~assets/img/tabbar/shopcart_active.svg">
      <div slot="item-text">购物车</div>
    </tab-bar-item>
    <tab-bar-item path='/profile' activeColor="deeppink">
      <img slot="item-icon" src="~assets/img/tabbar/profile.svg">
      <img slot="item-icon-active" src="~assets/img/tabbar/profile_active.svg">
        <div slot="item-text">我的</div>
    </tab-bar-item>
  </tab-bar>
</template>

<script>
  // 引入tabbar文件夹里的两个子组件
  import TabBar from 'components/common/tabbar/TabBar.vue'
  import TabBarItem from 'components/common/tabbar/TabBarItem.vue'

  export default {
    // 组件名称
    name: 'MainTabBar',
    components: {
      TabBar,
      TabBarItem
    }
  }
</script> 

<style scoped>

</style>