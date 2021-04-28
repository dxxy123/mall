<template>
  <div ref="wrapper">
    <div>
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
      scroll: {}
    }
  },
  methods: {
    // 封装，调用data里scroll的方法
    scrollTo(x, y, time) {  //(x, y, time)默认毫秒内滚到最上方 
      this.scroll && this.scroll.scrollTo(x, y, time)
    },
    // 重新刷新
    refresh() {
      this.scroll && this.scroll.refresh()
    },
    // 上拉加载更多
    finishPullUp() {
      this.scroll && this.scroll.finishPullUp()
    }
  },
  mounted() {
    // 1.创建BScroll对象
    this.scroll = new BScroll(this.$refs.wrapper, {
      click: true,  //div内的点击事件必须设置true才能正常点击
      observeDOM: true,  // 观察DOM的变化，封装插件后能正常滚动
      mouseWheel: true,  //鼠标滚轮控制滑动
      // preventDefault: false,
      probeType: this.probeType,
      pullUpLoad: this.pullUpLoad
    })

    // 2.监听实时滚动的位置
    if(this.probeType === 2 || this.probeType === 3) {
      this.scroll.on('scroll', (position) => {
        // 自定义事件把位置发送到首页或者其它页面，进行处理
        this.$emit('sendScroll', position)
      })
    }
    
    // 3.监听srcoll滚动到底部（上拉加载）
    if(this.pullUpLoad) {
      this.scroll.on('pullingUp',() => {
        this.$emit('pullingUP')
      })
    }   
  }
}
</script> 

<style scoped>

</style>