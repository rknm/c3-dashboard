<template>
  <div class="container">
    <div v-for="(data, index) in parsedData" :key="index">
      <div class="date">{{getYearAndMonth(data.InvoiceDate)}}</div>
      <pie v-bind:country-buy-data="data.countryBuyData" v-bind:country-data="data.countryData"></pie>
    </div>
  </div>
</template>

<script>
// hander のために必要
import Vue from "vue";

// 円グラフ描画コンポーネントを読み込む
import Pie from "./PieChart";

// CSS を読み込む
import "c3/c3.min.css";

// 国ごとの購入数
export default {
  name: "PurchaseCountry",
  components: {
    Pie
  },
  // htmlがコンポーネントが参照するプロパティ
  data: () => ({
    handler: new Vue()
  }),
  props: ["parsedData"],
  computed: {},
  mounted() {
    // DOMが全部作られてからコールされる
    // optionsで定義した値をc3へ渡す
    this.handler.$emit("init", this.options);
  },
  methods: {
    onPlotClick: function(d) {
      // 選択されたプロットのデータ名を親へ通知する
      this.$emit("emphasize", d.name);
    },
    getYearAndMonth: function(date) {
      return date.getFullYear() + "年" + (date.getMonth() + 1) + "月";
    }
  },
  watch: {
    // 親から渡された値が更新されたときに呼び出される
    emphasizeDataName: function(newVal) {
      this.handler.$emit("dispatch", chart => chart.focus(newVal));
    }
  }
};
</script>
<style scoped>
.container {
  display: grid;
  grid-auto-rows: 400px;
  grid-template-columns: repeat(auto-fit, 200px);
  justify-content: center; /* グリッド全体を中央に寄せる設定 */
}

.date {
  text-align: center;
  border-bottom: solid lightgray;
}
</style>
