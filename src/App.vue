<template>
  <div class="bg-black min-h-screen">
    <HomePage />
  </div>
</template>

<script>
import HomePage from "./pages/home.vue";
import "./assets/tailwind.css";

export default {
  name: "App",
  components: {
    HomePage,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
