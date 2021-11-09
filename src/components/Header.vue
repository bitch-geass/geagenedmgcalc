<template>
  <header>
    <h1>
      <a id="header-title" href="/">ダメージ計算β</a>
    </h1>
    <nav class="pc-nav">
      <ul>
        <li><a href="#">設定</a></li>
      </ul>
    </nav>
  </header>
</template>

<script>
</script>

<style scoped>
header {
  padding: 20px 20px 20px;
  position: fixed;
  top: 0;
  width: 95%;
  background-color: #4b4b4b;
  display: flex;
  align-items: center;
  box-sizing: border-box;
}
a#header-title {
  position: relative;
  left: 40px;
}
h1 {
  margin: 0;
  padding: 0;
  font-size: 20px;
}
a {
  text-decoration: none;
  color: #fff;
  bottom: 0;
}
ul {
  list-style: none;
  margin: 0;
  display: flex;
}
li {
  margin: 0 0 0 100px;
  font-size: 14px;
}
nav {
  margin: 0 0 0 auto;
}
</style>