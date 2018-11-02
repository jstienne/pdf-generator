<template>
  <div id="app">
    <img src="./assets/logo.png">
    <Generator/>
  </div>
</template>

<script>
import Generator from './components/Generator'

export default {
  name: 'App',
  components: {
    Generator
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

img {
  max-width: 10%;
  max-height: 10%;
}
</style>
