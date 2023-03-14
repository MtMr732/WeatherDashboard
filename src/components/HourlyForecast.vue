<script setup>
import Charts from './Charts.vue'
const props = defineProps({
  datalist: Array
})
</script>

<template>
  <div>
    <h2 v-if="props.datalist.length !== 0">HourlyForecast</h2>
    <Charts :datalist="props.datalist"> </Charts>
  </div>
</template>

<style scoped></style>
