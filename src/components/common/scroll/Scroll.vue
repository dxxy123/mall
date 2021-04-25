<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import BScroll from 'better-scroll'

export default {
  // 组件名称
  name: 'Scroll',
  props: {
    probeType: {
      type: Number,
      default: 0
    },
    pullUpLoad: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      scroll: null
    }
  },
  updated() {
    // 1.创建BScroll对象
    this.scroll = new BScroll(this.$refs.wrapper, {
      click: true,  //封装插件能正常点击
      observeDOM: true,  // 观察DOM的变化，封装插件后能正常滚动
      mouseWheel: true,  //鼠标滚轮控制滑动
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad
    })

    // 2.监听实时滚动的位置
    if(this.probeType === 2 || this.probeType === 3) {
      this.scroll.on('scroll', (position) => {
      // 自定义事件把位置传入到首页处理
      this.$emit('sendScroll', position)
    })
    }
    
    // 3.监听srcoll滚动到底部（上拉加载）
    if(this.pullUpLoad) {
      this.scroll.on('pullingUp',() => {
      this.$emit('pullingUP')
    })
    }
    
  },

  methods: {
    scrollTo(x, y) {  //(x, y, time=300)默认值500毫秒内滚到最上方
      this.scroll && this.scroll.scrollTo(x, y)
    },
    refresh() {
      this.scroll && this.scroll.refresh()
    },
    // 上拉加载更多
    finishPullUp() {
      this.scroll && this.scroll.finishPullUp()
    },
  }
}
</script> 

<style scoped>

</style>