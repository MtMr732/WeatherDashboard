<script setup>
import { inject, ref, toRefs, reactive, nextTick, watchEffect, onMounted } from 'vue'
// この中に、template内で利用する変数や関数を定義しておく。
import InputForm from './components/InputForm.vue'
import CurrentWeather from './components/CurrentWeather.vue'
import HourlyForecast from './components/HourlyForecast.vue'
import DailyForecast from './components/DailyForecast.vue'

const axios = inject('axios')
const API_KEY = import.meta.env.VITE_API_KEY
const baseURL = 'https://api.openweathermap.org/data/3.0/onecall'
const HOUR = 24
// 一時的にInputFormの中身をここに記載（componentに分けるときに削除する）
const lat = ref()
const lon = ref()
const resetFlag = ref(true)

let hourlyDataList = reactive([])
let dailyDataList = reactive([])
let currentData = reactive({
  dt: null,
  temp: null,
  weather: [
    {
      icon: '',
      description: ''
    }
  ]
})
const createDashboard = async () => {
  const data = await fetchData(
    `${baseURL}?lat=${lat.value}&lon=${lon.value}&exclude=minutely,alerts&appid=${API_KEY}&units=metric`
  )
  console.log(data)
  // APIで取得したデータを各ダッシュボード用に成形する関数
  format(data)

  // APIから取得したデータを下位コンポーネントに渡した後、v-ifの値を変更することでグラフを再レンダリングする処理
  resetFlag.value = false
  nextTick(() => (resetFlag.value = true))
}

// const useFetch = async (url) => {
//   const data = reactive({})
//   const dofetch = async (url) => {
//     data.value = null
//     await axios.get(url).then((res) => {
//       data = res.data
//     })
//   }
//   watchEffect(dofetch)
//   return {
//     data
//   }
// }

const fetchData = async (url) => {
  return await axios.get(url).then((res) => {
    return res.data
  })
}

const format = (data) => {
  currentData.dt = data.current.dt
  currentData.temp = data.current.temp
  currentData.weather[0].icon = data.current.weather[0].icon
  currentData.weather[0].description = data.current.weather[0].description

  let tempHourlyList = []
  for (let index = 0; index < HOUR; index++) {
    const { dt, temp } = data.hourly[index]
    const time = new Date(dt * 1000).getHours()
    tempHourlyList.push({ time, temp })
  }
  Object.assign(hourlyDataList, tempHourlyList)

  let tempDailyDataList = []
  for (let index = 0; index < data.daily.length; index++) {
    const { dt, temp } = data.daily[index]
    const date = new Date(Number(dt) * 1000)
    const { max, min } = temp
    const icon = data.daily[index].weather[0].icon
    // 新しい緯度経度でチャートを更新する時に前のデータが残ってしまうかもしれないので、要改善かも
    tempDailyDataList.push({ date, max, min, icon })
  }
  Object.assign(dailyDataList, tempDailyDataList)
}

onMounted(async () => {
  const data = await fetchData(
    `${baseURL}?lat=36&lon=140&exclude=minutely,alerts&appid=${API_KEY}&units=metric`
  )
  console.log(data)
  // APIで取得したデータを各ダッシュボード用に成形する関数
  format(data)

  // APIから取得したデータを下位コンポーネントに渡した後、v-ifの値を変更することでグラフを再レンダリングする処理
  resetFlag.value = false
  nextTick(() => (resetFlag.value = true))
})
</script>

<template>
  <main>
    <div class="title" style="display: flex,width: 40%">
      <h1>WeatherDashBoard</h1>
    </div>
    <div class="firstContainer">
      <div class="inputComponent">
        <InputForm>
          <input v-model="lat" type="text" placeholder="経度:lat" />
          <input v-model="lon" type="text" placeholder="緯度:lon" />
          <button @click="createDashboard">検索</button>
        </InputForm>
      </div>
      <div>
        <h2>Current Weather Data</h2>
        <CurrentWeather :data="currentData" />
      </div>
    </div>
    <div class="secondContainer">
      <DailyForecast :list="dailyDataList" />
      <HourlyForecast v-if="resetFlag" :datalist="hourlyDataList" />
    </div>
  </main>
</template>

<style>
img {
  margin-right: 10px;
}
.firstContainer {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;
}
.secondContainer {
  display: flex;
  flex-direction: column;
}
.inputComponent {
  display: flex;
  align-items: center;
  width: 40%;
}
button {
  min-width: 45px;
}
button:hover {
  cursor: pointer;
}

@media (min-width: 1024px) {
  .firstContainer {
    display: flex;
    flex-direction: row;
    align-items: stretch;
    justify-content: flex-start;
  }
  .secondContainer {
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
  }
}
</style>
