<template>
  <Header />
  <Menu />
  <BodyDmgCalc />
</template>

<script>
import Header from './components/Header.vue'
import Menu from './components/MenuBox.vue'
import BodyDmgCalc from './components/BodyDamageCalc.vue'

export default {
  name: 'App',
  components: {
    Header,
    Menu,
    BodyDmgCalc,
  }
}
</script>

<style>
#app {
  font-family: "Helvetica Neue",
    Arial,
    "Hiragino Kaku Gothic ProN",
    "Hiragino Sans",
    Meiryo,
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  margin-top: 60px;
}
</style>
