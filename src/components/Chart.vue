<script setup>
import Chart from 'chart.js/auto'
import { onMounted, watch } from 'vue'
const props = defineProps({
  datalist: Array
})
const labels = ['January', 'February', 'March', 'April', 'May', 'June']

const data = {
  labels: labels,
  datasets: [
    {
      label: 'My First dataset',
      backgroundColor: 'rgb(255, 99, 132)',
      borderColor: 'rgb(255, 99, 132)',
      data: props.datalist
    }
  ]
}

const config = {
  type: 'line',
  data: data,
  options: {}
}
watch(props.datalist, () => {
  new Chart(document.getElementById('chart'), config)
})

const click = () => {
  console.log(props.datalist)
}
</script>

<template>
  <button @click="click">孫ボタン</button>
  <canvas id="chart"></canvas>
</template>
