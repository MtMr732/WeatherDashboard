<script setup>
import Chart from './Chart.vue'
import Charts from './Charts.vue'
const props = defineProps({
  datalist: Array
})
const click = () => {
  console.log(props.datalist)
}

const dataList = props.datalist
</script>

<template>
  <div>
    <button @click="click">子ボタン</button>
    <!-- <Chart :datalist="props.datalist" ></Chart> -->
    <Charts :datalist="props.datalist"></Charts>
  </div>
  <ul>
    <li v-for="data in dataList">
      <!-- {{ data.dt }} -->
      {{ data.temp }}
    </li>
  </ul>
</template>

<style>
highcharts {
  width: 500px;
  height: 500px;
}
</style>
