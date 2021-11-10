<template>
<div>
  <Header />
  <Menu @switchPage="switchPage"/>
  <BodyDmgCalc v-if="isAtack" />
  <BodyDefenseCalc v-if="isDefense"/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Menu from './components/MenuBox.vue'
import BodyDmgCalc from './components/BodyDamageCalc.vue'
import BodyDefenseCalc from './components/BodyDefenseCalc.vue'
export default {
  name: 'App',
  components: {
    Header,
    Menu,
    BodyDmgCalc,
    BodyDefenseCalc,
  },
  data() {
      return {
        isAtack: true,
        isDefense: false,
      };
    },
  methods :{
    // ページ切り替え
    switchPage(pagename){
      switch(pagename){
        case "atack":
        default:
          this.isAtack = true;
          this.isDefense = false;
          break;
        case "defense":    
          this.isAtack = false;
          this.isDefense = true;
            break;
      }
    }
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