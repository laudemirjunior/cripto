<template>
  <HeaderComponent />
  <div class="container">
    <MajorCurrenciesComponent />
    <TrendingComponent />
  </div>
</template>

<script>
import MajorCurrenciesComponent from "../components/MajorCurrenciesComponent.vue";
import TrendingComponent from "../components/TrendingComponent.vue";
import HeaderComponent from "../components/HeaderComponent.vue";
export default {
  name: "HomePage",
  components: {
    MajorCurrenciesComponent,
    TrendingComponent,
    HeaderComponent,
  },
};
</script>

<style scoped>
.container {
  @apply flex w-11/12 m-auto gap-4 max-w-screen-2xl flex-col md:flex-row;
}
</style>
