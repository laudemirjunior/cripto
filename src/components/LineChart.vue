<template>
  <Line
    :chart-options="chartOptions"
    :chart-data="chartData"
    :chart-id="chartId"
    :dataset-id-key="datasetIdKey"
    :plugins="plugins"
    :css-classes="cssClasses"
    :styles="styles"
    :width="width"
    :height="height"
  />
</template>

<script>
import { Line } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale
);

export default {
  name: "LineChart",
  components: { Line },
  props: {
    date: Array,
    value: Array,
    chartCoin: String,
    chartId: {
      type: String,
      default: "Line-chart",
    },
    datasetIdKey: {
      type: String,
      default: "label",
    },
    width: {
      type: Number,
      default: 400,
    },
    height: {
      type: Number,
      default: 400,
    },
    cssClasses: {
      default: "",
      type: String,
    },
    styles: {
      type: Object,
      default: () => {
        Tooltip;
      },
    },
    plugins: {
      type: Object,
      default: () => {},
    },
  },
  data() {
    return {
      chartData: {
        labels: this.date,
        datasets: [
          {
            label: this.chartCoin,
            backgroundColor: "white",
            borderColor: "#6b21a8",
            data: this.value,
          },
        ],
      },
      chartOptions: {
        type: "line",
        responsive: true,
        maintainAspectRatio: false,
        elements: {
          point: {
            radius: 2,
          },
        },
        scales: {
          y: {
            grid: {
              color: "gray",
            },
          },
          x: {
            grid: {
              color: "gray",
            },
          },
        },
      },
    };
  },
};
</script>
