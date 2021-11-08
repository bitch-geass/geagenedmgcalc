<template>
    <header>
        <h1>
            <a href="/">ギアジェネダメージ計算ツール</a>
        </h1>
        <nav class="pc-nav">
            <ul>
                <li><a href="#">ダメージ計算</a></li>
                <li><a href="#">防御力計算</a></li>
                <li><a href="#">設定</a></li>
            </ul>
        </nav>
    </header>
</template>

<script>
</script>

<style scoped>
header {
    padding: 30px 2% 10px;
    position: fixed;
    top: 0;
    width: 95%;
    background-color: #4b4b4b;
    display: flex;
    align-items: center;
}
h1 {
   margin: 0; padding: 0;
   font-size: 20px;
}
a {	
   text-decoration: none;
   color: #FFF;
}
ul {
   list-style: none;
   margin: 0;
   display: flex;
}
li {
   margin: 0 0 0 15px;
   font-size: 14px;
}
nav {
   margin: 0 0 0 auto;
}
</style>