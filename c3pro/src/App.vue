<template>
  <div class="container">
    <!-- データの読み込みが終わるまでロード中を表示する -->
    <div class="loader" v-if="monthData.length === 0">Loading...</div>
    <div v-if="monthData.length !== 0">
      <!-- Salesに値を渡す -->
      <sales v-bind:parsed-data="parsedData"></sales>
      <season v-bind:parsed-data="monthData"></season>
      <country v-bind:parsed-data="monthData"></country>
      <prices v-bind:parsed-data="parsedData"></prices>
      <sold-and-sales v-bind:parsed-data="parsedData"></sold-and-sales>
      <transition v-bind:parsed-data="parsedData"></transition>
    </div>
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
//import csvData from "../Online_Retail_clipping.csv";
// CSV解析ライブラリ
const papa = require("papaparse");

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
    emphasizeDataName: "",
    parsedData: [],
    monthData: [],
    countryBuyData: [],
    yearsData: {}
  }),
  mounted() {
    // データをサーバから取得する
    fetch("http://127.0.0.1:8081/Online_Retail_clipping.csv")
      .then(res => res.text())
      .then(data => {
        this.parse(this, data);
      });
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
    },
    parse(obj, data) {
      papa.parse(data, {
        worker: true,
        header: true,
        fastMode: true,
        // CSVファイルを一行読むたびに実行される
        // row にはCSV1行分のデータが含まれている
        step: function(row) {
          // データが持つ日付が有効な場合のみ処理する
          if (
            row.data.InvoiceDate &&
            // 正規表現でdateが正しいものだけを選択する
            row.data.InvoiceDate.match(
              /^(\d\d\d\d)-(\d\d)-(\d\d) (\d\d):(\d\d):(\d\d)$/
            ) !== null
          ) {
            // 季節ごとの売上変化用と国ごとの売上データを作成
            obj.createSeasonDate(obj, row);
          }
        },
        complete: function() {
          console.log("complete");
          // 生成したオブジェクトをグラフ向けのデータに加工する
          for (let [key, value] of Object.entries(obj.yearsData)) {
            // グラフで日付を扱えるようにDate型に変換する
            value["InvoiceDate"] = new Date(key);
            obj.monthData.push(value);

            // 国ごとの売り上げを円グラフ表示できるように
            // 国名の配列を作成する
            const countryData = [];
            for (let key of Object.keys(value.countryBuyData)) {
              countryData.push(key);
            }
            value["countryData"] = countryData;
          }
        }
      });
    },
    createSeasonDate(obj, row) {
      const date = new Date(row.data.InvoiceDate);
      const yearAndMonth = date.getFullYear() + "-" + (date.getMonth() + 1);
      if (!Object.prototype.hasOwnProperty.call(obj.yearsData, yearAndMonth)) {
        obj.yearsData[yearAndMonth] = {
          totalUnitPrice: 0,
          totalQuantity: 0
        };
      }
      // c3js側で計算できるように数値に変換
      obj.yearsData[yearAndMonth].totalUnitPrice += Number.parseFloat(
        row.data.UnitPrice
      );
      obj.yearsData[yearAndMonth].totalQuantity += Number.parseFloat(
        row.data.Quantity
      );

      // 国ごとの売上個数データを作成
      if (
        !Object.prototype.hasOwnProperty.call(
          obj.yearsData[yearAndMonth],
          "countryBuyData"
        )
      ) {
        obj.yearsData[yearAndMonth]["countryBuyData"] = {};
      }

      const buyData =
        obj.yearsData[yearAndMonth].countryBuyData[row.data.Country] || 0;
      obj.yearsData[yearAndMonth].countryBuyData[row.data.Country] =
        buyData + Number.parseFloat(row.data.Quantity);
    }
  }
};
</script>

<style>
.container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(800px, 1fr));
}
.loader,
.loader:before,
.loader:after {
  background: #c0c0c0;
  -webkit-animation: load1 1s infinite ease-in-out;
  animation: load1 1s infinite ease-in-out;
  width: 1em;
  height: 4em;
}
.loader {
  color: #c0c0c0;
  text-indent: -9999em;
  margin: 88px auto;
  position: relative;
  font-size: 11px;
  -webkit-transform: translateZ(0);
  -ms-transform: translateZ(0);
  transform: translateZ(0);
  -webkit-animation-delay: -0.16s;
  animation-delay: -0.16s;
}
.loader:before,
.loader:after {
  position: absolute;
  top: 0;
  content: "";
}
.loader:before {
  left: -1.5em;
  -webkit-animation-delay: -0.32s;
  animation-delay: -0.32s;
}
.loader:after {
  left: 1.5em;
}
@-webkit-keyframes load1 {
  0%,
  80%,
  100% {
    box-shadow: 0 0;
    height: 4em;
  }
  40% {
    box-shadow: 0 -2em;
    height: 5em;
  }
}
@keyframes load1 {
  0%,
  80%,
  100% {
    box-shadow: 0 0;
    height: 4em;
  }
  40% {
    box-shadow: 0 -2em;
    height: 5em;
  }
}
</style>
