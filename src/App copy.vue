<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Apple v-on:fluitTotal="totalDayo()"></Apple>
    <Orange v-on:fluitTotal="totalDayo()"></Orange>
    <p>果物は全部で{{ total }}個だよ</p>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import Orange from './components/Orange.vue'
import Apple from './components/Apple.vue'

export default {
  name: 'app',
  components: {
    HelloWorld,
    Orange,
    Apple
  },
  data:() =>({
    total: 0
  }),
  methods: {
    totalDayo: function() {
      this.total++
    }
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
