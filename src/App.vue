<template>
  <div>
    <Header />
    <Menu />
    <!--body-->
    <router-view id="body"/>
  </div>
</template>
<script>
import Header from '@/components/Header.vue'
import Menu from '@/components/MenuBox.vue'
export default {
  name: 'App',
  components: {
    Header,
    Menu,
  },
}
</script>
<style>
#body {
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
  padding-top: 65px; /* 固定ヘッダー分ずらす */
  box-sizing: border-box;
}

</style>
