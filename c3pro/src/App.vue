<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <chart v-for="type in chartTypeList" v-bind:key="type" @emphasize="emphasize" v-bind:chart-type="type" v-bind:chart-data="chartData" v-bind:emphasize-data-name="emphasizeDataName" v-bind:x-ticks="xTicks"></chart>
  </div>
</template>

<script>
import Chart from './components/Chart.vue'
//import axios from '../node_modules/axios'

export default {
  name: 'App',
  components: {
    Chart
  },
  // htmlがコンポーネントが参照するプロパティ
  data: () => ({
    // 表示するチャートの型を指定
    chartTypeList: ["line", "bar", "pie"],
    // chartData: [["x", 0, 20, 10, 40, 15, 500], ["y", 30, 200, 100, 400, 150, 250]],
    // ["宇都宮", 516546, 518398, 519904, 521082, 521702, 521556], ["埼玉", 7209945, 7223317, 7239813, 7270549, 7291207, 7310697]
    chartData: [["宇都宮", 516546, 518398, 519904, 521082, 521702, 521556], ["ダミー", 506546, 508398, 509904, 511082, 511702, 521556]],
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
}
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
