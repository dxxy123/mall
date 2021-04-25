<template>
  <div id="home">
    <!-- 顶部导航栏 -->
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <!-- 可滚动区域 -->
    <scroll class="content"
            ref="scroll" 
            :probe-type="3" @sendScroll="contentScroll" 
            :pull-up-load="true" @pullingUP="loadMore">
      <!-- 轮播图 -->
      <home-swiper :banners="banners"></home-swiper>
      <!-- 中间推荐版块 -->
      <recommend-view :recommends="recommends"></recommend-view>
      <!-- 流行版块 -->
      <feature-view></feature-view>
      <tab-control  
              :titles="['流行', '新款', '精选']"
              @tabClick="tabClick">
    </tab-control>
      <!-- 商品分类 -->
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <!-- 向上滚到最顶部按钮 -->
    <!-- <back-top @click.native="backClick" v-show="isShowBackTop"></back-top> -->
    <back-top @click.native="backClick"></back-top>
  </div>
</template>

<script>
// 导入公共组件
import NavBar from 'components/common/navbar/NavBar'
import TabControl from'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/common/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'

// 导入子组件
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import {getHomeMultidata, getHomeGoods} from 'network/home'

export default {
  // 组件名称
  name: 'Home',
  components: {
    NavBar,
    TabControl,
    GoodsList,
    Scroll,
    BackTop,
    HomeSwiper,
    RecommendView,
    FeatureView
  },
  data() {
    return {
      banners: [],
      recommends: [],
      goods: {
        'pop': {page: 0, list: []},
        'new': {page:0, list: []},
        'sell': {page: 0, list: []}
      },
      currentType: 'pop',
      // isShowBackTop: false,
    }
  },

  computed: {
    showGoods() {
      return this.goods[this.currentType].list
    }
  },
  created() {
    // 1.请求轮播图的多个数据
    this.getHomeMultidata()

    // 2.请求商品列表的数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  mounted() {
    // 1.监听item中图片加载完成
    this.$bus.$on('itemImageLoad', () => {
      this.scroll && this.$refs.scroll.refresh()
    })

    // 2.获取tabControl
  },

  methods: {
    /* 事件监听相关的方法 */
    tabClick(index) {
      switch(index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType = 'sell'
          break
      }
    },
    backClick() {
      this.$refs.scroll.scrollTo(0, 0)  //在默认值内滚到最上方
    },
    // 监听首页里需要实时检测滚动的图片部分
    contentScroll(position) {
      this.isShowBackTop = (-position.y) > 1500
    },
    // 上拉加载更多
    loadMore() {
      // 监听图片加载完成，记录当前选中的类型
      this.getHomeGoods(this.currentType)
    },

    /* 网络请求相关的方法 */
    getHomeMultidata() {
      getHomeMultidata().then(res => {
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    },
    // 请求商品图片的数据和页数
    getHomeGoods(type) {
      let page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
        this.goods[type].list.push(...res.data.list)  //遍历获取的数据
        this.goods[type].page += 1
        // 发起第二次图片数据请求,完成上拉加载更多
        this.$refs.scroll.finishPullUp()
      })
    }
  }
}
</script>

<style scoped>
  #home {
    padding-top: 44px;
    /* height: 100vh; */
    /* position: relative; */
  }
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /* 使用原生滚动时，为了导航不跟随一起滚动 */
    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
  }


  /* 计算中间的滚动区域高度 */
  .content {
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }
  /* .content {
    height: calc(100% - 93px);
    overflow: hidden;
    margin-top: 44px;
  } */
</style>