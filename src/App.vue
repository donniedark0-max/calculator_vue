<template>
  <div id-="app">
    <Calculator />
  </div>
</template>

<script>
import Calculator from './components/calculator/calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #171616; /* Color de fondo del app */
}
</style>
