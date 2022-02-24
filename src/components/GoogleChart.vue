<template>
  <div>
    <GChart
        v-if="data_loaded"
        type="LineChart"
        :options="options"
        :data="chartData"
    />
    <div v-else>
      Loading Chart data...
    </div>
  </div>
</template>

<script>
import {GChart} from "vue-google-charts";

const API = 'https://api.ridian.io/fund_performances';
export default {
  name: "App",
  props: ['low', 'mid', 'high'],
  components: {
    GChart
  },
  data() {
    return {
      benchmarks: {},
      wanted_funds: {},
      options: {
        curveType: "nonen",
        chartArea: {width: "80%", height: "65%"},
        backgroundColor: "#212121",
        height: 369,
        legend: {
          position: "top",
          textStyle: {color: "white"},
        },
        hAxis: {
          textStyle: {color: "white"},
        },
        vAxis: {
          textStyle: {color: "white"},
        },
        series: {
          0: {color: "#f1ca3a"},
          1: {color: "#e7711b"},
          2: {color: "#e2431e"},
          3: {color: "#6f9654"},
          4: {color: "#1c91c0"},
          5: {color: "#43459d"},
        },
      }
    };
  },
  computed: {
    data_loaded() {
      return Object.keys(this.benchmarks).length
    },
    navs() {
      if (!this.low || !this.mid || !this.high) return {};
      if (Object.keys(this.benchmarks).length === 0) return {};
      return {
        BTC: this.benchmarks["BTC"],
        SPY: this.benchmarks["SPY"],
        low: this.wanted_funds[this.low].performance,
        mid: this.wanted_funds[this.mid].performance,
        high: this.wanted_funds[this.high].performance,
      };
    },
    chartData() {
      if (Object.keys(this.navs).length === 0) return [];
      const dates = Object.keys(this.navs["low"]).sort();
      const data = dates.map((date) => {
        return [
          date,
          this.navs["low"][date],
          this.navs["mid"][date],
          this.navs["high"][date],
          this.navs["BTC"][date],
          this.navs["SPY"][date],
        ];
      });
      const filtered_data = [];
      for (let i = 0; i < data.length; i++) {
        if (i % 5 === 0) {
          filtered_data.push(data[i]);
        }
      }
      filtered_data.unshift([
        "date",
        "Low Risk",
        "Mid Risk",
        "High Risk",
        "BTC",
        "SPY",
      ]);
      return filtered_data;
    }
  },
  mounted() {
    fetch(API)
        .then((res) => res.json())
        .then((res) => {
          this.benchmarks = res.benchmarks;
          this.wanted_funds = res.wanted_funds;
        })
  }
};
</script>
<style>
</style>
