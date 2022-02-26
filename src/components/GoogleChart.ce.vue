<template>
  <div class="chart-web-component">
    <div class="chart-content" v-if="data_loaded">
      <ul class="buttons-list">
        <li>
          <button class="active">BTC/ETH</button>
        </li>
        <li>
          <button>TOP5</button>
        </li>
        <li>
          <button>TOP10</button>
        </li>
      </ul>
      <GChart class="chart"
              type="LineChart"
              :options="options"
              :data="chartData"
      />
      <div class="risk-management">
        <input id="auto_risk_management" type="checkbox">
        <label for="auto_risk_management">Automatic Risk Management</label>
      </div>
    </div>
    <div v-else>
      Loading Chart data...
    </div>
  </div>
</template>

<script>
import {GChart} from "vue-google-charts";
import dayjs from "dayjs";

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
          position: "bottom",
          alignment: 'start',
          textStyle: {color: "white"},
        },
        hAxis: {
          textStyle: {color: "white", fontSize: 10},
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
          dayjs(date).format('DD MMM'),
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
.chart-web-component {
  display: flex;
  flex-direction: column;
  align-items: stretch;
}

.chart-content {
  display: flex;
  align-self: stretch;
  flex-direction: column;
  position: relative;
}

.buttons-list {
  list-style: none;
  display: flex;
  align-self: center;
  background-color: #171926;
  padding: 5px;
  border-radius: 10px;
  margin: 0 0 5px;
}

.buttons-list button {
  border: none;
  padding: 10px;
  border-radius: 10px;
  background-color: transparent;
  color: #717380;
}

.buttons-list button.active {
  background-color: #1375EA;
  color: white;
}

.chart {
  flex-grow: 1;
}

.risk-management {
  position: absolute;
  bottom: 10px;
  right: 200px;
  color: rgba(255, 255, 255, .7);
}
</style>
