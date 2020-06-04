<template>
  <div class="table-container">
    <div class="cell header">オーダー数</div>
    <div class="cell header">売上</div>
    <div class="cell">{{parsedData.length}}</div>
    <div class="cell">{{0}}</div>
  </div>
</template>

<script>
// hander のために必要
import Vue from "vue";

// CSS を読み込む
import "c3/c3.min.css";

// 国ごとの販売数及び売り上げ
export default {
  name: "SoldAndSales",
  // htmlがコンポーネントが参照するプロパティ
  data: () => ({
    handler: new Vue(),
    sales: 0
  }),
  props: ["parsedData"],
  computed: {
    // 親から渡されたデータから売上を計算する
    // calcSales: () => {
    //   console.log(this);
    //   let result = 0;
    //   // if (!this.parsedData) {
    //   //   return result;
    //   // }
    //   // for (let i = 0; i < this.parsedData.length; i++) {
    //   //   const obj = this.parsedData[i];
    //   //   result += parseFloat(obj.Quantity) * parseFloat(obj.UnitPrice);
    //   // }
    //   return result;
    // }
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
    },
    parsedData: {
      immediate: true,
      handler: function() {
        let result = 0;
        if (!this.parsedData) {
          return;
        }
        // TODO 計算に時間がかかりすぎる
        for (let i = 0; i < this.parsedData.length; i++) {
          // const obj = this.parsedData[i];
          // result += parseFloat(obj.Quantity) * parseFloat(obj.UnitPrice);
          // console.log("in");
        }
        this.sales = result;
      }
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
  align-items: center;
  justify-content: center;
  font-size: 2vh;
}

.header {
  background-color: lightgray;
}
</style>
