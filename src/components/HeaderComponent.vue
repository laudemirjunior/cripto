<template>
  <header class="container">
    <div class="main">
      <a class="flex items-center">
        <img
          src="https://assets.coingecko.com/coins/images/1/large/bitcoin.png?1547033579"
          alt="logo"
          class="w-10"
        />

        <span class="ml-3 font-bold">Cripto</span>
      </a>
      <a>
        <span class="font-bold">Home</span>
      </a>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderComponent",
};
</script>

<style scoped>
.container {
  @apply text-gray-100 bg-black w-full max-w-screen-2xl m-auto shadow-purple-700 shadow-md;
}
.main {
  @apply flex items-center flex-row justify-between w-11/12 m-auto pt-2 pb-2;
}
</style>
