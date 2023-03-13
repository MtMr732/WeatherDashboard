<script setup>
import { inject, ref, nextTick } from 'vue'
// この中に、template内で利用する変数や関数を定義しておく。
import InputForm from './components/InputForm.vue'
import CurrentWeather from './components/CurrentWeather.vue'
import HourlyForecast from './components/HourlyForecast.vue'
import DailyForecast from './components/DailyForecast.vue'
import { provide } from 'vue'

const axios = inject('axios')
const API_KEY = import.meta.env.VITE_API_KEY
const baseURL = 'https://api.openweathermap.org/data/3.0/onecall'
const iconBaseURL = 'https://openweathermap.org/img/wn/' // iconIdの後に@2x.pngが必要
const HOUR = 24
// 一時的にInputFormの中身をここに記載（componentに分けるときに削除する）
const lat = ref(36)
const lon = ref(140)
const resetFlag = ref(true)

let hourlyDataList = ref([])
let dailyDataList = ref([])
let currentData = ref({
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
  const data = await fetchData()
  console.log(data)
  // APIで取得したデータを各ダッシュボード用に成形する関数
  format(data)

  // APIから取得したデータを下位コンポーネントに渡した後、v-ifの値を変更することでグラフを再レンダリングする処理
  resetFlag.value = false
  nextTick(() => (resetFlag.value = true))
}

// const useFetch = () => {
//   const data = ref(null)
//   const dofetch = async() => {
//     data.value = null
//     await axios
//       .get(
//         `${baseURL}?lat=${lat.value}&lon=${lon.value}&exclude=minutely,alerts&appid=${API_KEY}&units=metric`
//       )
//       .then((res) => {
//         data.value = res.data
//       })
//   }
//   dofetch()

//   return {
//     data
//   }
// }
// watch(click,()=>{
//   dofetch()
// })

const fetchData = async () => {
  return await axios
    .get(
      `${baseURL}?lat=${lat.value}&lon=${lon.value}&exclude=minutely,alerts&appid=${API_KEY}&units=metric`
    )
    .then((res) => {
      return res.data
    })
}

const format = (data) => {
  // let tempList = ref([])
  // let dailyDataList = ref([])
  currentData.value.dt = data.current.dt
  currentData.value.temp = data.current.temp
  currentData.value.weather[0].icon = data.current.weather[0].icon
  currentData.value.weather[0].description = data.current.weather[0].description

  const hourlyData = data.hourly
  const dailyData = data.daily

  for (let index = 0; index < HOUR; index++) {
    const { dt, temp } = hourlyData[index]
    const time = new Date(dt * 1000).getHours()
    // 新しい緯度経度でチャートを更新する時に前のデータが残ってしまうかもしれないので、要改善かも
    hourlyDataList.value.push({ time, temp })
  }
  // console.log(tempList)
  // hourlyDataList = tempList
  for (let index = 0; index < dailyData.length; index++) {
    const { dt, temp } = dailyData[index]
    const { max, min } = temp
    const icon = dailyData[index].weather[0].icon
    // 新しい緯度経度でチャートを更新する時に前のデータが残ってしまうかもしれないので、要改善かも
    dailyDataList.value.push({ dt, max, min, icon })
  }
  // return {
  //   hourlyDataList,
  //   dailyDataList
  // }
}

const click = () => {
  console.log(hourlyDataList)
}
</script>

<template>
  <header></header>

  <main>
    <div class="firstContainer">
      <button @click="click">親</button>
      <InputForm @click="createDashboard">
        <input v-model="lat" type="text" placeholder="経度:lat" />
        <input v-model="lon" type="text" placeholder="緯度:lon" />
        <button @click="createDashboard">検索</button>
      </InputForm>
      <CurrentWeather :data="currentData" />
    </div>
    <div class="secondContainer">
      <HourlyForecast v-if="resetFlag" :datalist="hourlyDataList" />
      <DailyForecast :list="dailyDataList" />
    </div>
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

  .firstContainer {
    display: flex;
    flex-direction: row;
    align-items: stretch;
    justify-content: center;
  }
  .secondContainer {
    display: flex;
    justify-content: space-evenly;
  }
}
</style>
