<template>
  <div>
    <!-- TODO v-for で月ごとの円グラフを作る -->
    <vue-c3 :handler="handler"></vue-c3>
  </div>
</template>

<script>
// hander のために必要
import Vue from "vue";

// メインのコンポーネント
import VueC3 from "vue-c3";

// CSS を読み込む
import "c3/c3.min.css";

// 国ごとの購入数
export default {
  name: "PurchaseCountry",
  components: {
    VueC3
  },
  // htmlがコンポーネントが参照するプロパティ
  data: () => ({
    handler: new Vue()
  }),
  props: ["chartType", "chartData", "emphasizeDataName", "xTicks"],
  computed: {
    // c3 のオプションに渡す値
    // ここでC3特有の値を渡す
    options() {
      return {
        // 主としてデータ
        data: {
          // 円グラフ
          type: "pie",
          // columns: this.chartData,
          columns: [
            // 配列一つ目が、データの名前になる
            ["日本", 0, 20, 10, 40, 15, 250],
            ["アメリカ", 30, 200, 100, 400, 150, 250]
          ]
        },
        pie: {
          label: {
            show: false
          }
        }
      };
    }
  },
  mounted() {
    // DOMが全部作られてからコールされる
    // optionsで定義した値をc3へ渡す
    this.handler.$emit("init", this.options);
  }
};
</script>
<style scoped>
</style>
