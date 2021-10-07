<template>
  <div id="app">
    <img alt="Cegedim logo" class="logo" src="./assets/logo.png">
    <NumbersApp />
  </div>
</template>

<script>
import NumbersApp from './components/NumbersApp.vue'

export default {
  name: 'App',
  components: {
    NumbersApp
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

.logo {
  width: 300px;
  margin-bottom: 50px;
}
</style>
