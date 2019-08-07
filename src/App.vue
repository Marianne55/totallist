<template>
  <div id="app">
    <HelloWorld/>
    <todulist/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import todulist from './components/todulist'

export default {
  name: 'App',
  components: {
    HelloWorld,
    todulist
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
