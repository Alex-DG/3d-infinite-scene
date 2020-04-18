<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <Infinite />
  </div>
</template>

<script>
import Infinite from './components/Infinite.vue'

export default {
  name: 'App',
  components: {
    Infinite
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
