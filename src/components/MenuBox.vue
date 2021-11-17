<template>
  <!-- ↓ハンバーガーメニューここから↓ -->
  <div class="h-menubox">
    <input id="hamburger" type="checkbox" class="input-hidden" />
    <label for="hamburger" class="h-switch h-switch1">
      <span class="hamburger-switch-line1"></span>
    </label>
    <div class="h-menuwrap">
      <section>
        <!--a id="menu-title">ギアジェネ</a-->
        <ul class="h-menulist">
          <hr />
          <li id="li-home"><a>ホーム</a></li>
          <hr />
          <li id="li-01" class="menu-hover">
            <a href="#" @click="clickAtack"
              ><img src="../assets/atack.png" />&nbsp;&nbsp;ダメージ計算</a
            >
          </li>
          <li id="li-02">
            <a href="#" @click="clickDefense"
              ><img src="../assets/defense.png" />&nbsp;&nbsp;防御力計算</a
            >
          </li>
          <hr />
        </ul>
      </section>
    </div>
    <div class="h-cover"></div>
  </div>
  <!-- ↑ハンバーガーメニューここまで↑ -->
</template>

<script>
export default {
  methods: {
    applyHoverStyle(idname) {
      // 選択色の削除と適用
      var elements = document.getElementsByClassName("menu-hover");
      if (elements.length > 0) {
        elements[0].classList.remove("menu-hover"); // どうせ1つなので先頭だけ削除
      }
      document.getElementById(idname).classList.add("menu-hover");
      // ついでにハンバーガーメニューも閉じる
      document.getElementById("hamburger").checked = false;
    },
    clickAtack() {
      this.applyHoverStyle("li-01");
      //★子側のデータ渡し口（$emit）
      this.$emit("switchPage", "atack");
    },
    clickDefense() {
      this.applyHoverStyle("li-02");
      //★子側のデータ渡し口（$emit）
      this.$emit("switchPage", "defense");
    },
  },
};
</script>

<style scoped>
/* 全体調整CSS */
.h-menubox * {
  font-size: 16px;
}
.h-menubox li {
  font-size: 14px;
}
/* hamburgerここから */
/* inputを非表示 */
.input-hidden {
  display: none;
}
/* label */
.h-switch {
  cursor: pointer;
  position: absolute;
  left: 0;
  top: 0; /* ハンバーガーアイコンの位置（上から） */
  z-index: 9999;
  width: 4em; /* アイコン（クリック可能領域）の幅 */
  height: 4em; /* アイコン（クリック可能領域の）高さ */
  position: fixed;
}
/* ハンバーガーアイコン */
.hamburger-switch-line1,
.hamburger-switch-line1:before,
.hamburger-switch-line1:after {
  width: 25px;
  height: 3px;
  background: rgb(236, 231, 231); /* ハンバーガーアイコンの色 */
  position: absolute;
  top: 50%;
  left: 50%;
  transition: 0.3s;
  content: "";
}
.hamburger-switch-line1 {
  transform: translate(-50%, -50%);
}
.hamburger-switch-line1:before {
  transform: translate(-50%, -300%);
}
.hamburger-switch-line1:after {
  transform: translate(-50%, 200%);
}
/* ハンバーガーアイコン･アニメーション */
#hamburger:checked ~ .h-switch .hamburger-switch-line1 {
  width: 0;
}
#hamburger:checked ~ .h-switch .hamburger-switch-line1:before {
  transform: rotate(45deg) translate(-40%, 325%);
}
#hamburger:checked ~ .h-switch .hamburger-switch-line1:after {
  transform: rotate(-45deg) translate(-40%, -325%);
}
/* メニューエリア･アニメーション */
/* 右から */
#hamburger:checked ~ .h-menuwrap {
  right: 60%;
}
/* コンテンツカバー */
#hamburger:checked ~ .h-cover {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 9997;
  background: rgba(3, 3, 3, 0.5);
  display: block;
}

/* メニューエリア */
.h-menuwrap {
  position: fixed;
  height: 100%;
  background: #4b4b4b; /* メニューエリアの背景色 */
  padding: 40px 0 0 0;
  z-index: 9998;
  transition: 0.3s;
  overflow-y: scroll; /* メニュー内容が多い場合に縦スクロールする */
  top: 0;
  right: 100%;
  width: 40%;
}
/* メニューリスト */
.h-menulist {
  margin-right: 0%;
  padding-left: 0% !important; /* !important不要な場合もあり */
  list-style: none;
}
/* MENU 選択時の色 */
.menu-hover {
  background-color: rgba(255, 255, 255, 0.24);
  color: white;
}
.h-menulist li a {
  display: block;
  text-decoration: none;
  color: #fff; /* メニューリストの文字色 */
  text-align: left;
  padding: 0 0 0 20px;
  line-height: 40px;
}
.h-menulist li a img {
  transform: translateX(-50%);
  transform: translateY(30%);
  width: 30px;
}
.h-menulist hr {
  border-width: 1px 0 0 0;
  margin: 5px 0;
}
#li-home a {
  /*font-size: 20px;*/
  color: #fff; /* メニューリストの文字色 */
  text-align: center;
  padding: 0;
}
</style>