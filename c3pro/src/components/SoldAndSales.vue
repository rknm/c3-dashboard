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

// 国ごとの販売数及び売り上げ
export default {
  name: "SoldAndSales",
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
          // 棒グラフ
          type: "bar",
          columns: this.chartData,
          // columns: [
          //   // 配列一つ目が、データの名前になる
          //   ["x", 0, 20, 10, 40, 15, this.val],
          //   ["y", 30, 200, 100, 400, 150, 250]
          // ]
          // selection: {
          //   enabled: true
          // },
          onclick: (d, element) => {
            // console.log(d);
            // console.log(element);
            this.onPlotClick(d, element);
          }
        },
        axis: {
          x: {
            label: {
              // X 軸のラベルを変更する
              // 位置も変えれる
              text: "年",
              position: "outer-center"
            },
            type: "category",
            categories: this.xTicks
          },
          y: {
            label: {
              text: "人口数",
              position: "outer-middle"
            }
          }
        },
        // tooltip: {
        //   // 各点にマウスオーバーした際に出てくる部分の設定
        //   format: {
        //     title(d) {
        //       return `第 ${d} 期`;
        //     },
        //     value(value, ratio, id) {
        //       return `${value} 億円`;
        //     }
        //   }
        // },
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
