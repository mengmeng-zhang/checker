<template>
  <div id="app">
    <Background/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Background from './components/background'

export default {
  name: 'App',
  components: {
    HelloWorld,
    Background
  }
}
</script>

<style>
@import './assets/css/reset.scss';
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
