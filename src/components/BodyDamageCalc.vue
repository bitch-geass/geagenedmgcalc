<template>
  <div id="app">
    <!--攻撃側-->
    <div class="container-status">
      <table class="table-status">
        <colgroup>
          <col />
          <col />
        </colgroup>
        <tbody>
          <tr>
            <th>キャラ攻撃力</th>
            <td>
              <div>
                <input
                  v-model="inAtack"
                  type="tel"
                  @input="calcAtk()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>コード攻撃力</th>
            <td>
              <div>
                <input
                  v-model="inAtackCl"
                  type="tel"
                  @input="calcAtk()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>攻撃倍率(%)</th>
            <td>
              <div>
                <input
                  v-model="inRate"
                  type="tel"
                  @input="calcRate()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>攻撃バフ(%)</th>
            <td>
              <div>
                <input
                  v-model="inAtackbuf"
                  type="tel"
                  @input="calcAtk()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>クリバフ(%)</th>
            <td>
              <div>
                <input
                  v-model="inCribuf"
                  type="tel"
                  @input="calcCribuf()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>攻撃回数</th>
            <td>
              <div>
                <input
                  v-model="inHits"
                  type="tel"
                  @input="calcDmg()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--防御側-->
    <div class="container-status-enm">
      <table class="table-status">
        <tbody>
          <tr>
            <th>敵防御力</th>
            <td>
              <div>
                <input
                  v-model="inDf"
                  type="tel"
                  @input="calcDf()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>防御バフ(%)</th>
            <td>
              <div>
                <input
                  v-model="inDfbuf"
                  type="tel"
                  @input="calcDf()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>防御デバフ(%)</th>
            <td>
              <div>
                <input
                  v-model="inDfDebuf"
                  type="tel"
                  @input="calcDf()"
                  class="input-control"
                />
              </div>
            </td>
          </tr>
          <tr>
            <th>実防御力</th>
            <td>{{ defense }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--ダメージ-->
    <div class="container-calc">
      <table class="table-calc">
        <thead>
          <tr>
            <th>ダメージ</th>
            <th>クリダメージ</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{{ dmg }}</td>
            <td>{{ cridmg }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--dmg計算式-->
    <div class="container-calc">
      <table class="table-calc">
        <thead>
          <tr>
            <th>攻撃力</th>
            <th>係数</th>
            <th>倍率</th>
            <th>クリ倍率</th>
            <th>回数</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{{ atkSum }}</td>
            <td><p id="td-coeff">{{ coeff }}</p></td>
            <td>{{ rate }}</td>
            <td>{{ cribuf }}</td>
            <td>{{ inHits }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inAtack: 4825,
      inAtackCl: 1800,
      inAtackbuf: 120,
      atackbuf: 0,
      atkSum: 0,
      inDf: 3950,
      inDfbuf: 0,
      inDfDebuf: 0,
      defense: 0,
      coeff: 0,
      inRate: 505,
      rate: 0,
      inCribuf: 120,
      cribuf: 0,
      inHits: 1,
      dmg: 0,
      cridmg: 0,
    };
  },
  created: function () {
    this.calcAtk();
    this.calcDf();
    this.calcRate();
    this.calcCribuf();
  },
  methods: {
    calcAtk() {
      //空白ならゼロとか。
      this.atkSum = (
        (1.0 + parseInt(this.inAtackbuf) / 100.0) *
        (parseInt(this.inAtack) + parseInt(this.inAtackCl))
      ).toFixed(1);
      this.calcCoeff();
    },
    calcDf() {
      //TODO バフとデバフの順序が不明
      this.defense = (
        (1 + parseInt(this.inDfbuf) / 100 - parseInt(this.inDfDebuf) / 100) *
        parseInt(this.inDf)
      ).toFixed(1);
      this.calcCoeff();
    },
    calcCoeff() {
      this.coeff = this.getCoeff(this.atkSum, this.defense).toFixed(3);
      this.calcDmg();
    },
    getCoeff(atk, def) {
      var tmpCoeff = atk / (def * 5);
      if (tmpCoeff > 2) {
        tmpCoeff = 2;
      } else if (this.coeff < 0.1) {
        tmpCoeff = 0.1;
      }
      return tmpCoeff;
    },
    calcRate() {
      this.rate = this.getRate(this.inRate).toFixed(3);
      this.calcDmg();
    },
    getRate(inRate){
      // 同盟施設で1.1倍
      return (parseInt(inRate) * 1.1) / 100;
    },
    calcCribuf() {
      // クリティカル150％ + 同盟施設で10％追加
      this.cribuf = this.getCribuf(this.inCribuf).toFixed(2);
      this.calcDmg();
    },
    getCribuf(inCribuf) {
      // クリティカル150％ + 同盟施設で10％追加
      return (parseInt(inCribuf) + 160) / 100;
    },
    calcDmg() {
      var tmpDmg = (this.atkSum * this.getCoeff(this.atkSum, this.defense) * this.getRate(this.inRate)) / this.inHits;
      this.dmg = tmpDmg.toFixed(0);
      this.cridmg = (tmpDmg * this.getCribuf(this.inCribuf)).toFixed(0);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.app {
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
}
/* ステータス */
.container-status {
  display: inline-block;
  padding: 16px 1%;
  text-align: left;
}
.container-status table {
  border-collapse: separate;
  border-spacing: 0px 1px;
  margin: 0 auto;
}
.container-status td,
th {
  padding: 1px;
  width: 100px;
}
.container-status th {
  background: #2695c5fd;
  color: #fff;
}
.container-status td {
  background: #f1fafe;
}
/* 敵ステータステーブル */
.container-status-enm {
  display: inline-block;
  padding: 16px 1%;
  text-align: left;
  vertical-align: top;
}
.container-status-enm table {
  border-collapse: separate;
  border-spacing: 0px 1px;
  margin: 0 auto;
}
.container-status-enm td,
th {
  padding: 1px;
  width: 100px;
}
.container-status-enm th {
  background: #e97b93fd;
  color: #fff;
}
.container-status-enm td {
  background: #f1fafe;
}

/* 計算テーブル */
.container-calc {
  padding: 16px 1%;
  text-align: center;
}
.container-calc table {
  align-content: left;
  border-collapse: separate;
  border-spacing: 0px 1px;
  margin: 0 auto;
}
.container-calc td,
th {
  padding: 5px;
}
.container-calc th {
  background: #0a101385;
  color: #fff;
}
.container-calc td {
  background: #f1fafe;
}

.table {
  table-layout: fixed;
}
.table tr td {
  vertical-align: middle;
  word-wrap: break-word;
}

input {
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
  /*
  overflow: hidden;
  width: 48px;
  font-size: 16pt;
  -webkit-appearance: none;
  border-radius: 0;*/
}
</style>
