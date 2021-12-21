<template>
  <div id="app">
    <VirtualList :size="40" :items="10" :arrayData="list"/>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    VirtualList: ()=>import('./component/virtual-list.vue'),
  },
  data () {
    return {
      list:[],
    }
  },
  created () {
    for (let i = 0; i < 10000; i++) {
      this.list.push({
        id: i,
        val: `我是第【${i+1}】个数据~`
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
