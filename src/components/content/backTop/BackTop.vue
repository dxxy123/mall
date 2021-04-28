<template>
  <div class="back-top" @click="topClick">
    <img src="~assets/img/common/top.png">
  </div>
</template>

<script>
  export default {
    // 组件名称
    name: 'BackTop',
    methods: {
      topClick: function () {
        this.$emit('backTop')
      }
    }

}
</script> 

<style scoped>
  .back-top {
    position: fixed;
    right: 10px;
    bottom: 60px;

  }
  .back-top img {
    width: 43px;
    height: 43px;
  }

</style>