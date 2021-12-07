<template>
  <test-component-2/>
  
</template>

<script>
import testComponent2 from './components/testComponent2.vue'

export default {
  name: 'App',
  components: {
    testComponent2
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
