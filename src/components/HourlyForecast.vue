<script setup>
import Charts from './Charts.vue'
const props = defineProps({
  datalist: Array
})
</script>

<template>
  <div>
    <Charts :datalist="props.datalist"></Charts>
  </div>
</template>

<style>
highcharts {
  width: 500px;
  height: 500px;
}
</style>
