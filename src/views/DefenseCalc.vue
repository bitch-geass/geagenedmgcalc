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
              <th>攻撃力</th>
              <td>
                <div>
                  <input
                    v-model="inAtack"
                    type="tel"
                    maxlength="4"
                    @input="calcDefense()"
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
                    @input="calcDefense()"
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
                    @input="calcDefense()"
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
                    @input="calcDefense()"
                    class="input-control"
                  />
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
      <!--実ダメージ入力欄-->
      <div class="container-status-enm">
        <table class="table-status">
          <tbody>
            <tr>
              <th>実ダメージLow</th>
              <td>
                <div>
                  <input
                    v-model="inDmgLow"
                    type="tel"
                    maxlength="7"
                    @input="calcDefense()"
                    class="input-control"
                  />
                </div>
              </td>
            </tr>
            <tr>
              <th>実ダメージHigh</th>
              <td>
                <div>
                  <input
                    v-model="inDmgHigh"
                    type="tel"
                    maxlength="7"
                    @input="calcDefense()"
                    class="input-control"
                  />
                </div>
              </td>
            </tr>
            <tr>
              <th>誤差(％)2.5％以内</th>
              <td>
              <p id="td-coeff">{{ dmgError }}</p>
            </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!--計算防御力-->
    <div class="container-calc">
      <table class="table-calc">
        <tr>
          <th>予想防御力</th>
          <td>{{ defense }}</td>
        </tr>
        <tr>
          <th>係数</th>
          <td>{{ coeff }}</td>
        </tr>
      </table>
      <p>誤差2.5％に近づくほど正確な値を出せる。2.5％を超える場合には入力値がおかしい。(バフやデバフがかかってしまっている可能性大)
      </p>
      <p>現在は係数の上限下限の考慮はなし。2を超えても計算できるか？</p>    
      <p>扇が1回攻撃で、育てていない人が多いのでおススメ</p>
    </div>
  </div>
</template>

<script>
export default {
  title: '防御力計算',
  data() {
    return {
      inAtack: 1376,
      inAtackCl: 0,
      inRate: 100,
      inHits: 1,
      inDmgLow: 888,
      inDmgHigh: 926,
      dmgError: 0,
      defense: 0,
      coeff: 0,
    };
  },
  methods: {
    calcDefense() {
      var atkSum = Number(this.inAtack) + Number(this.inAtackCl);
      // 同盟施設で固定10パーとする
      var rate =(Number(this.inRate) + 10) / 100;
      // ダメージ系
      var dmglow;
      var dmgHigh;
      dmglow = (this.inDmgLow.length != 0) ? Number(this.inDmgLow) : Number(this.inDmgHigh);
      dmgHigh = (this.inDmgHigh.length != 0) ? Number(this.inDmgHigh) : Number(this.inDmgLow);

      var dmgCenter = (dmglow + dmgHigh) / 2;
      this.dmgError = (100 * (dmgHigh - dmgCenter)/ dmgCenter).toFixed(2);

      // 防御力計算
      var tmpDefense = atkSum * atkSum * rate
                / (5 * this.inHits * dmgCenter);

      this.defense = tmpDefense.toFixed(1)

      // 係数
      this.coeff = (atkSum / (5*tmpDefense)).toFixed(2);
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
  padding: 0.3em;
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
