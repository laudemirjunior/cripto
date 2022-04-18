<template>
  <div class="container">
    <div class="main">
      <form class="flex gap-2 flex-col" @submit.prevent="convertForUnix">
        <h1 class="">{{ coin }}</h1>
        <div class="flex justify-between">
          <input
            required
            v-model="dateTime"
            type="datetime-local"
            class="border border-purple-700 w-8/12"
          />
          <button class="btn-primary" type="submit">Buscar</button>
        </div>
      </form>

      <div class="button">
        <button class="btn-primary" @click="close">Fechar</button>
        <div v-if="this.coinDate.prices">
          <p>
            {{
              newDate(
                convertForDate(coinDate.prices[coinDate.prices.length - 1][0])
              )
            }}
          </p>
          <p>{{ price(coinDate.prices[coinDate.prices.length - 1][1]) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "@/service";
export default {
  name: "ModalComponent",
  data() {
    return {
      dateTime: "",
      coinDate: {},
    };
  },
  props: {
    close: Function,
    coin: String,
  },
  methods: {
    async getCripto(unix) {
      await api
        .get(
          `https://api.coingecko.com/api/v3/coins/${this.coin.toLowerCase()}/market_chart/range?vs_currency=brl&from=${
            unix - 1200
          }&to=${unix + 1200}`
        )
        .then((res) => (this.coinDate = res.data));
    },
    convertForUnix() {
      const dateStr = this.dateTime + ":00.000Z";
      const newDate = new Date(dateStr).valueOf();
      this.getCripto((newDate + 11200000) / 1000);
      console.log(this.coin);
    },
    convertForDate(value) {
      const newValue = value;
      return new Date(newValue);
    },
    price(value) {
      return value.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL",
      });
    },
    newDate(date) {
      var dt = new Date(date);
      return dt.toLocaleString().split(" ").reverse().join(" ");
    },
  },
};
</script>

<style scoped>
.container {
  @apply bg-gray-500 bg-opacity-75 transition-opacity w-full min-h-full fixed top-0 left-0 flex justify-center items-center;
}
.main {
  @apply bg-white h-60 flex flex-col justify-center p-4 m-4 border border-purple-700 rounded-lg gap-10 w-96 relative;
}
.button {
  @apply w-full flex flex-row-reverse justify-between;
}
</style>
