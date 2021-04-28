<template>
  <div id="home">
    <!-- 顶部导航栏 -->
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <!-- 可滚动区域 -->
    <scroll class="content"
            ref="scroll" 
            :probe-type="3" 
            @sendScroll="contentScroll" 
            :pull-up-load="true" 
            @pullingUP="loadMore">
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
    <!-- 向上滚到最顶部 -->
    <back-top @backTop="backTop" v-show="showBackTop"></back-top>
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
  import {NEW, POP, SELL, BACKTOP_DISTANCE} from "common/const"

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
        currentType: POP,
        showBackTop: false,
      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    methods: {
      /* 事件监听相关的方法 */

      // 防抖动（避免首页图片频繁刷新）
      debounce(func, delay) {
        let timer = null
        return function(...args) {
          if(timer) clearTimeout(timer)
          timer = setTimeout(() => {
            func.apply(this, args)
          }, delay)
        }
      },
      // 流行、新款、精选 版块的显示选择
      tabClick(index) {
        switch(index) {
          case 0:
            this.currentType = POP
            break
          case 1:
            this.currentType = NEW
            break
          case 2:
            this.currentType = SELL
            break
        }
      },
      // backTop是否显示（实时监听首页里滚动的图片，接收scroll里发出的position）
      contentScroll(position) {
        this.showBackTop = position.y < -BACKTOP_DISTANCE
      },
      // 上拉加载更多（监听图片加载完成，当前选中的类型）
      loadMore() {
        this.getHomeGoods(this.currentType)
      },
      // 滚到顶部，调用scroll组件里的方法
      backTop() {
        this.$refs.scroll.scrollTo(0, 0, 200)  
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
    },
    created() {
      // 1.请求轮播图的多个数据
      this.getHomeMultidata()

      // 2.请求商品列表的数据
      this.getHomeGoods(POP)
      this.getHomeGoods(NEW)
      this.getHomeGoods(SELL)
    },
    mounted() {
      // 1.监听item中图片加载完成
      const refresh = this.debounce(this.$refs.scroll.refresh, 200)
      this.$bus.$on('itemImageLoad', () => {
        // this.scroll && this.$refs.scroll.refresh()
        refresh()
      })

      // 2.获取tabControl
    },
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