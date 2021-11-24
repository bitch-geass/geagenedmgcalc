<template>
  <div>
    <!--攻撃側-->
    <div id="status--">
      <a>ボーナス値をスペース区切りで入力してね。(例)</a>
      <input v-model="inBonus" maxlength="100" class="input-control" />
      <div>
        <button @click="opBonus">計算</button>
      </div>
      <div class="container-calc">
        <table class="table-calc">
          <tr>
            <th>部隊1</th>
            <td>{{ unit1 }}</td>
          </tr>
          <tr>
            <th>部隊2</th>
            <td>{{ unit2 }}</td>
          </tr>
          <tr>
            <th>部隊3</th>
            <td>{{ unit3 }}</td>
          </tr>
          <tr>
            <th>部隊4</th>
            <td>{{ unit4 }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  title: "ダメージ計算",
  data() {
    return {
      inBonus: "40,13,12,8,6,6",
      inCodeBonus: "6,7,5",
      unit1: "?",
      unit2: "?",
      unit3: "?",
      unit4: "?",
    };
  },
  methods: {
    opBonus() {
      // 区切り文字で区切って数値ソート(降順)
      var arrayBouns = this.inBonus
        .split(",")
        .map(Number)
        .sort(this.comparefunction);

      //
      this.splitBonus(arrayBouns, 50);
    },
    splitBonus(arrayBouns, maxBonu) {
      var tmpBonu = 0;
      var array1 = new Array(0);
      var array2 = new Array(0);
      for (let i = 0; i < arrayBouns.length - 1; i++) {
        tmpBonu += arrayBouns[i];
        array1.push(arrayBouns[i]);
        array2.shift
        if (tmpBonu > maxBonu) {
          alert(maxBonu + "超えたよ：" + array1);
        }
      }
    },
    // 数値を降順ソートする関数
    comparefunction(a, b) {
      return b - a;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* テーブルのスタイル全体 */
* table {
  table-layout: auto;
  white-space: nowrap; /* 自動改行しない */
}
* th,
td {
  vertical-align: middle;
  word-wrap: normal;
  padding: 1px;
}
* th {
  background: #0a101385;
  color: white;
  padding: 1px 4px 0 4px;
}
* td {
  background: #f1fafe;
}
* input {
  font: 15px/24px sans-serif;
  box-sizing: border-box;
  width: 100%;
  padding: 0.3em;
  transition: 0.3s;
  letter-spacing: 1px;
  color: #2e2d2dd7;
  border: 1px solid #ffffff;
  box-shadow: 1px 1px 2px 0 #707070 inset;
  border-radius: 4px;
  text-align: left;
}
/* ステータス入力テーブルのスタイル */
#status {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}
/* ステータス */
.container-status {
  display: inline-block;
  text-align: left;
}
/* ヘッダーのスタイル */
.container-status th {
  background: #2695c5fd;
  color: #fff;
  width: 110px;
}
/* セルのスタイル */
.container-status td {
  background: #f1fafe;
  width: 60px;
}

/* 敵ステータステーブル */
.container-status-enm {
  display: inline-block;
  text-align: left;
  vertical-align: top;
}
.container-status-enm th {
  background: #e97b93fd;
  color: #fff;
  width: 100px;
}
.container-status-enm td {
  background: #f1fafe;
  width: 60px;
}
</style>
