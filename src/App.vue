<script setup>
import { inject } from 'vue'
// この中に、template内で利用する変数や関数を定義しておく。
import InputForm from './components/InputForm.vue'
</script>

<template>
  <header></header>

  <main>
    <InputForm />
    <CurrentWeather />
    <HourlyForecast />
    <DailyForecast />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
