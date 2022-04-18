<template>
  <div v-if="date.length !== 0">
    <h1 class="title">Últimas 24 horas</h1>
    <LineChart :date="date" :value="value" :chartCoin="chartCoin" />
  </div>
</template>

<script>
import { api } from "../service";
import LineChart from "../components/LineChart.vue";

export default {
  name: "ChartCoinComponent",
  data: function () {
    return {
      showModal: false,
      date: [],
      value: [],
    };
  },
  components: {
    LineChart,
  },
  props: {
    chartCoin: String,
  },
  methods: {
    async getChartCoin() {
      this.date = [];
      this.value = [];
      let newDate = [];
      let newValue = [];
      await api
        .get(`coins/${this.chartCoin}/market_chart?vs_currency=brl&days=1`)
        .then((res) => {
          res.data.prices.forEach((item) =>
            newDate.push(this.newDateChart(item[0]))
          );
          res.data.prices.forEach((item) => newValue.push(item[1]));
        });
      this.date = newDate;
      this.value = newValue;
    },
    newDateChart(date) {
      var dt = new Date(date);
      return dt.toLocaleString().split(" ")[1];
    },
  },
  created() {
    this.getChartCoin();
  },
  watch: {
    chartCoin() {
      this.getChartCoin();
    },
  },
};
</script>
