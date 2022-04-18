<template>
  <div class="container">
    <h1 class="title">Principais moedas</h1>
    <div class="main">
      <div
        v-for="(item, index) in sortName(dataCriptos)"
        :key="index"
        class="card"
      >
        <div class="image">
          <img :src="item.image" class="w-10" />
        </div>
        <div class="container-data">
          <div class="w-6/12 sm:w-5/12">
            <p>{{ item.name }}</p>
            <p>({{ item.symbol }})</p>
          </div>
          <div class="w-full sm:w-5/12">
            <p>{{ price(item.current_price) }}</p>
            <p>{{ newDate(item.last_updated) }}</p>
          </div>
        </div>
        <div class="buttons">
          <button class="btn-primary" @click="chartCoin = item.id">
            Gráfico
          </button>
          <button class="btn-primary" @click="close(), (coin = item.name)">
            Buscar
          </button>
        </div>
      </div>
    </div>
    <ChartCoinComponent :chartCoin="chartCoin" />
  </div>
  <ModalComponent v-if="showModal" :close="close" :coin="coin" />
</template>

<script>
import { api } from "@/service";
import ModalComponent from "./ModalComponent.vue";
import ChartCoinComponent from "./ChartCoinComponent.vue";
export default {
  name: "MajorCurrenciesComponent",
  data() {
    return {
      criptos: ["bitcoin", "cosmos", "dacxi", "ethereum", "terra-luna"],
      dataCriptos: [],
      showModal: false,
      chartCoin: "bitcoin",
    };
  },
  components: {
    ModalComponent,
    ChartCoinComponent,
  },
  methods: {
    getCripto(cripto) {
      api
        .get(
          `coins/markets?vs_currency=brl&ids=${cripto}&order=market_cap_desc&per_page=100&page=1&sparkline=false`
        )
        .then((res) => this.dataCriptos.push(res.data[0]));
    },
    updateAllCriptos() {
      this.criptos.forEach((item) => {
        api
          .get(
            `coins/markets?vs_currency=brl&ids=${item}&order=market_cap_desc&per_page=100&page=1&sparkline=false`
          )
          .then((res) => {
            let oldData = this.dataCriptos.find(
              (item) => item.name === res.data[0].name
            );
            oldData.last_updated = res.data[0].last_updated;
            oldData.current_price = res.data[0].current_price;
          });
      });
    },
    getAllCriptos() {
      this.criptos.forEach((item) => {
        this.dataCriptos = [];
        this.getCripto(item);
      });
    },
    price(value) {
      return (
        "R$ " +
        value.toLocaleString("pt-BR", {
          currency: "BRL",
        })
      );
    },
    newDate(date) {
      var dt = new Date(date);
      return dt.toLocaleString().split(" ").reverse().join(" ");
    },
    sortName(data) {
      return data.sort((a, b) => a.name.localeCompare(b.name));
    },
    close() {
      this.showModal = !this.showModal;
    },
  },
  created() {
    this.getAllCriptos();
    setInterval(() => {
      this.updateAllCriptos();
    }, 10000);
  },
};
</script>

<style scoped>
.container {
  @apply flex flex-col md:w-9/12 w-full;
}
.main {
  @apply flex flex-col gap-2 w-full;
}
.card {
  @apply flex flex-col sm:flex-row items-center bg-black border border-purple-800 text-white shadow-purple-700 shadow-md p-2 w-full gap-5;
}
.image {
  @apply w-2/12 flex items-center justify-center;
}
.container-data {
  @apply flex w-full items-center justify-center;
}
.buttons {
  @apply w-full sm:w-6/12 flex items-center justify-center gap-4;
}
</style>
