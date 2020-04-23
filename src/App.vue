<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />
    <h1>3D Infinite Scene - Vue, Three & Typescript</h1>
    <section>
      <p>Left click: Zoom In/Zoom Out</p>
    </section>
    <section>
      <Infinite />
    </section>
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
}
body {
  margin: 0;
  padding: 10px 0;
}
img {
  height: 130px;
}
</style>
