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
  props: ["parsedData"],
  computed: {
    // c3 のオプションに渡す値
    // ここでC3特有の値を渡す
    options() {
      return {
        // データのフォーマットを指定する
        data: {
          // 棒グラフ
          type: "bar",
          // 対象データの型を指定する
          json: this.parsedData,
          // x軸とy軸のデータ名を指定する
          keys: {
            x: "itemName",
            value: ["totalSales", "totalQuantity"]
          }
        },
        axis: {
          rotated: true,
          x: {
            label: {
              text: "商品名",
              position: "outer-middle"
            },
            type: "category"
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
.table-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  padding-bottom: 10p;
  height: 100px;
}

.cell {
  border: solid 1px #ddd;
  display: flex;
  text-align: center;
}
</style>
