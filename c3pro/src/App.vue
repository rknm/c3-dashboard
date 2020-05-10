<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <chart
      v-for="type in chartTypeList"
      v-bind:key="type"
      @emphasize="emphasize"
      v-bind:chart-type="type"
      v-bind:chart-data="chartData"
      v-bind:emphasize-data-name="emphasizeDataName"
      v-bind:x-ticks="xTicks"
    ></chart>-->
    <sales></sales>
    <season></season>
    <country></country>
    <prices></prices>
    <sold-and-sales></sold-and-sales>
    <transition></transition>
  </div>
</template>

<script>
// 今年度売上
import Sales from "./components/Sales";
// 季節ごとの売り上げ変化
import Season from "./components/SeasonalSales";
// 国ごとの購入数
import Country from "./components/PurchasesCountry";
// 単価と売上
import Prices from "./components/PriceAndSales";
// 商品ごとの販売数および売上
import SoldAndSales from "./components/SoldAndSales";
// 商品ごとの売り上げ推移
import Transition from "./components/SalesTransition";

export default {
  name: "App",
  components: {
    Sales,
    Season,
    Country,
    Prices,
    SoldAndSales,
    Transition
  },
  // htmlがコンポーネントが参照するプロパティ
  data: () => ({
    // TODO チャートが表示するデータを整形して各チャートに渡す
    // 表示するチャートの型を指定
    // chartTypeList: ["line", "bar", "pie"],
    // chartData: [["x", 0, 20, 10, 40, 15, 500], ["y", 30, 200, 100, 400, 150, 250]],
    // ["宇都宮", 516546, 518398, 519904, 521082, 521702, 521556], ["埼玉", 7209945, 7223317, 7239813, 7270549, 7291207, 7310697]
    chartData: [
      ["宇都宮", 516546, 518398, 519904, 521082, 521702, 521556],
      ["ダミー", 506546, 508398, 509904, 511082, 511702, 521556]
    ],
    xTicks: ["H25", "H26", "H27", "H28", "H29", "H30"],
    emphasizeDataName: ""
  }),
  mounted() {
    // TODO データをサーバから取得する
    // axios.defaults.baseURL = 'http://localhost:8080';
    // axios.defaults.headers.get['Content-Type'] = 'application/json;charset=utf-8';
    // axios.defaults.headers.get['Access-Control-Allow-Origin'] = '*';
    // axios
    //   .get('https://www.google.com/')
    //   .then(() => this.chartData.push(["x", 0, 20, 10, 40, 15, 500], ["y", 30, 200, 100, 400, 150, 250]))
    //   .catch(() => this.chartData.push(["x", 0, 20, 10, 40, 15, 500], ["y", 30, 200, 100, 400, 150, 250]))
  },
  methods: {
    emphasize(name) {
      // 子から通知されたデータ名を保持する
      // ここで値を変更することで、強調表示する値を子へ通知する
      if (this.emphasizeDataName === name) {
        this.emphasizeDataName = "";
      } else {
        this.emphasizeDataName = name;
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
