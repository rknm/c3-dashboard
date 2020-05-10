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

// 季節ごとの売り上げ変化
export default {
  name: "SeasonalSales",
  components: {
    VueC3
  },
  // htmlがコンポーネントが参照するプロパティ
  data: () => ({
    handler: new Vue()
  }),
  //props: ["chartType", "chartData", "emphasizeDataName", "xTicks"],
  computed: {
    // c3 のオプションに渡す値
    // ここでC3特有の値を渡す
    options() {
      return {
        // 主としてデータ
        data: {
          // x軸に表示するデータを指定
          // columns内の配列を指定する
          x: "x",
          // 棒グラフと線グラフ
          types: {
            data1: "bar",
            data2: "line"
          },
          // データ名を設定
          names: {
            data1: "売上",
            data2: "オーダー数"
          },
          // 画面左と右のY軸名を設定
          axes: {
            data1: "y",
            data2: "y2"
          },
          //columns: this.chartData,
          columns: [
            // 配列一つ目がdata.xで指定した値と一致しているため
            // 2つめ以降の値がx軸で表示される
            // TODO 年月の配列を作る必要あり
            [
              "x",
              "2010-12-01",
              "2011-1-01",
              "2011-2-01",
              "2011-3-01",
              "2011-4-01",
              "2011-5-01",
              "2011-6-01",
              "2011-7-01",
              "2011-8-01",
              "2011-9-01",
              "2011-10-01",
              "2011-11-01",
              "2011-12-01"
            ],
            // TODO ここから下のデータは月ごとの売り上げとオーダー数に分ける
            ["data1", 30, 200, 100, 400, 150, 250],
            ["data2", 50, 300, 200, 500, 250, 350]
          ],
          onclick: (d, element) => {
            this.onPlotClick(d, element);
          }
        },
        axis: {
          x: {
            type: "timeseries",
            tick: {
              format: "%Y年%m月",
              rotate: 90
            }
            //categories: this.xTicks
          },
          y: {
            // 画面左のy軸ラベル
            label: {
              text: "売り上げ",
              position: "outer-middle"
            }
          },
          y2: {
            show: true,
            // 画面右のy軸ラベル
            label: {
              text: "オーダー数",
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
