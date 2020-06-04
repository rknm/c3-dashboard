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
  props: ["countryBuyData", "countryData"],
  computed: {
    // c3 のオプションに渡す値
    // ここでC3特有の値を渡す
    options() {
      return {
        // 主としてデータ
        data: {
          // 円グラフ
          type: "pie",
          json: [this.countryBuyData],
          keys: {
            value: this.countryData
          }
        },
        legend: {
          show: false
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
