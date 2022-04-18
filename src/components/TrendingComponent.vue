<template>
  <div class="w-full md:w-3/12 pb-4">
    <h1 class="title">Tendência</h1>
    <div class="main">
      <div class="cards">
        <div v-for="(item, index) in trending" :key="index" class="card">
          <div class="flex items-center gap-4">
            <img :src="item.item.large" class="w-10 bg-white rounded-full" />
            <p>{{ item.item.name }}</p>
            <p>({{ item.item.symbol }})</p>
          </div>
          <div>
            <p>BTC: {{ item.item.price_btc }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { api } from "@/service";
export default {
  name: "TrendingComponent",
  data: function () {
    return {
      trending: [],
    };
  },
  methods: {
    getTrending() {
      api.get("search/trending").then((res) => {
        this.trending = res.data.coins;
      });
    },
  },
  created() {
    this.getTrending();
  },
};
</script>

<style>
.main {
  @apply flex flex-col gap-4 w-full;
}
.cards {
  @apply flex flex-col gap-2 w-full;
}
.card {
  @apply bg-black border border-purple-800 text-white shadow-purple-700 shadow-md text-sm flex flex-col gap-4 p-2;
}
</style>
