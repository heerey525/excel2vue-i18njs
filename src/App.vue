<template>
  <div id="app">
    <h1>多语言生成</h1>
    <p>eg.上传多语言excel文件,下载多语言js文件</p>
    <Language />
  </div>
</template>

<script>
import Language from './components/Language.vue'

export default {
  name: 'app',
  components: {
    Language
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
