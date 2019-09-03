<template>
  <div id="app">
    <Api/>
  </div>
</template>

<script>
import Api from './components/Api.vue'

export default {
  name: 'app',
  components: {
    Api
  }
}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
body {
  background-image: linear-gradient(135deg, #fdfcfb 0%, #e2d1c3 100%);
}
</style>
