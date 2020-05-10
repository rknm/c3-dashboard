<template>
  <div>
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

// 単価と売上
export default {
  name: "PriceAndSales",
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
          // 散布図
          // 1つのグラフに複数表示する
          types: {
            data1: "scatter",
            data2: "scatter"
          },
          // データ名を設定
          names: {
            data1: "a",
            data2: "b"
          },
          //columns: this.chartData,
          columns: [
            // 配列一つ目が、データの名前になる
            ["data1", 0, 20, 10, 40, 15, 100],
            ["data2", 30, 200, 100, 400, 150, 250]
          ]
        },
        axis: {
          y: {
            label: {
              text: "売上",
              position: "outer-middle"
            }
          }
        },
        grid: {
          x: {
            show: true
          },
          y: {
            show: true
          }
        },
        legend: {
          position: "right"
        }
      };
    }
  },
  mounted() {
    // DOMが全部作られてからコールされる
    // optionsで定義した値をc3へ渡す
    this.handler.$emit("init", this.options);
  },
  methods: {
    onPlotClick: function(d) {
      // 選択されたプロットのデータ名を親へ通知する
      this.$emit("emphasize", d.name);
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
.button-wrapper {
  text-align: center;
}

button {
  padding: 10px;
  background: red;
}
</style>
