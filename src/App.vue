<template>
  <Header></Header>
  <BodyDmgCalc />
</template>

<script>
import Header from './components/Header.vue'
import BodyDmgCalc from './components/BodyDamageCalc.vue'

export default {
  name: 'App',
  components: {
    Header,
    BodyDmgCalc
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
  margin: 0;
  padding: 0;
  margin-top: 60px;
}
</style>
