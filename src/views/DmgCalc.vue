<template>
  <div>
    <!--攻撃側-->
    <div id="status">
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
                    maxlength="4"
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
                    maxlength="4"
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
                    maxlength="4"
                    @input="calcRate()"
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
                    maxlength="1"
                    @input="calcDmg()"
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
                    maxlength="4"
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
                    maxlength="4"
                    @input="calcCribuf()"
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
                    maxlength="5"
                    @input="calcDf()"
                    class="input-control"
                  />
                </div>
              </td>
            </tr>
            <tr>
              <th>防バフ(%)</th>
              <td>
                <div>
                  <input
                    v-model="inDfbuf"
                    type="tel"
                    maxlength="4"
                    @input="calcDf()"
                    class="input-control"
                  />
                </div>
              </td>
            </tr>
            <tr>
              <th>防デバフ(%)</th>
              <td>
                <div>
                  <input
                    v-model="inDfDebuf"
                    type="tel"
                    maxlength="3"
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
            <tr>
              <th>属性有利</th>
              <td>
                <div>
                  <input
                    v-model="inIsAdvantage"
                    type="checkbox"
                    @change="calcDmg()"
                    class="checkbox"
                  />
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!--ダメージ-->
    <div class="container-calc">
      <table class="table-calc">
        <tr>
          <th>ダメージ</th>
          <td>{{ dmg }}</td>
        </tr>
        <tr>
          <th>クリダメージ</th>
          <td>{{ cridmg }}</td>
        </tr>
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
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>{{ atkSum }}</td>
            <td>{{ coeff }}</td>
            <td>{{ rate }}</td>
            <td>{{ cribuf }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  title: 'ダメージ計算',
  data() {
    return {
      inAtack: 6600,
      inAtackCl: 1800,
      inAtackbuf: 131,
      atackbuf: 0,
      atkSum: 0,
      inDf: 4350,
      inDfbuf: 0,
      inDfDebuf: 0,
      inIsAdvantage: true,
      defense: 0,
      coeff: 0,
      inRate: 550,
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
        (1.0 + Number(this.inAtackbuf) / 100.0) *
        (Number(this.inAtack) + Number(this.inAtackCl))
      ).toFixed(1);
      this.calcCoeff();
    },
    calcDf() {
      //TODO バフとデバフの順序が不明
      this.defense = (
        (1 + Number(this.inDfbuf) / 100 - Number(this.inDfDebuf) / 100) *
        Number(this.inDf)
      ).toFixed(1);
      if (this.defense < 0) {
        this.defense = 0;
      }
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
      } else if (tmpCoeff < 0.1) {
        tmpCoeff = 0.1;
      }
      return tmpCoeff;
    },
    calcRate() {
      this.rate = this.getRate(this.inRate).toFixed(3);
      this.calcDmg();
    },
    getRate(inRate) {
      // 同盟施設で1.1倍
      return (Number(inRate) * 1.1) / 100;
    },
    calcCribuf() {
      // クリティカル150％ + 同盟施設で10％追加
      this.cribuf = this.getCribuf(this.inCribuf).toFixed(2);
      this.calcDmg();
    },
    getCribuf(inCribuf) {
      // クリティカル150％ + 同盟施設で10％追加
      return (Number(inCribuf) + 160) / 100;
    },
    calcDmg() {
      var tmpDmg =
        (this.atkSum *
          this.getCoeff(this.atkSum, this.defense) *
          this.getRate(this.inRate)) /
        this.inHits;
      
      if(this.inIsAdvantage){
        tmpDmg = tmpDmg * 1.5;
      }
      this.dmg = this.getDmgRange(tmpDmg);
      this.cridmg = this.getDmgRange(tmpDmg * this.getCribuf(this.inCribuf));
    },
    getDmgRange(dmg) {
      return (
        Number((dmg * 0.975).toFixed(0)).toLocaleString() +
        "～" +
        Number((dmg * 1.025).toFixed(0)).toLocaleString()
      );
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
* th,td {
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
  padding: 0.2em;
  transition: 0.3s;
  letter-spacing: 1px;
  color: #2e2d2dd7;
  border: 1px solid #ffffff;
  box-shadow: 1px 1px 2px 0 #707070 inset;
  border-radius: 4px;
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
  width: 100px;
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
.checkbox {
    box-sizing: border-box;
    cursor: pointer;
    display: inline-block;
    padding: 5px 30px;
    position: relative;
    width: auto;
}
</style>
