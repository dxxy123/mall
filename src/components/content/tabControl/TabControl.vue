<template>
  <div class="tab-control">
    <div v-for="(item, index) in titles" :key="index" 
      class="tab-control-item" 
      :class="{active: index === currentIndex}"
      @click="itemClick(index)">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
export default {
  // 组件名称
  name: 'TabControl',
  props: {
    titles: {
      type: Array,
      default() {
        return []
      }
    }
  },
  data() {
    return {
      currentIndex: 0
    }
  },
  methods: {
    itemClick(index) {
      // 获取控件里的当前点击
      this.currentIndex = index
      // 把控件里的点击传到首页
      this.$emit('tabClick', index)

    }
  }
}
</script> 

<style scoped>
  .tab-control {
    display: flex;
    text-align: center;
    font-size: 16px;
    height: 40px;
    line-height: 40px;
    background-color: #fff;
  }
  .tab-control-item {
    flex: 1;
  }
  .tab-control-item span{
    padding: 5px;
  }
  .active {
    color: var(--color-high-text)
  }
  .active span {
    border-bottom: 3px solid var(--color-tint)
  }
</style>