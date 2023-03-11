<script setup>
import { inject, ref } from 'vue'
// この中に、template内で利用する変数や関数を定義しておく。
import InputForm from './components/InputForm.vue'

const API_KEY = import.meta.env.VITE_API_KEY
const baseURL = 'https://api.openweathermap.org/data/3.0/onecall'
const axios = inject('axios')

// 一時的にInputFormの中身をここに記載（componentに分けるときに削除する）
const lat = ref()
const lon = ref()
const clickSubmit = () => {
  console.log('lat:' + lat.value, 'lon:' + lon.value)
}

let formatedData = {}
const fetchData = () => {
  axios
    .get(
      `${baseURL}?lat=${lat.value}&lon=${lon.value}&exclude=minutely,alerts&appid=${API_KEY}&units=metric`
    )
    .then((res) => {
      console.log(res.data)
    })
  // APIで取得したデータを各ダッシュボード用に成形する関数
  // format(data);
}
const format = (data) => {
  const currentData = data.current
  const hourlyData = data.hourly
  const dailyData = data.daily

  formatedData = { currentData, hourlyData, dailyData }
}
</script>

<template>
  <header></header>

  <main>
    <button @click="fetchData">クリック</button>

    <input v-model="lat" type="text" placeholder="経度:lat" />
    <input v-model="lon" type="text" placeholder="緯度:lon" />
    <button @click="clickSubmit">更新</button>

    <!-- <InputForm /> -->
    <!-- <CurrentWeather />
    <HourlyForecast />
    <DailyForecast /> -->
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
