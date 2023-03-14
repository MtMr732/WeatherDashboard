<script setup>
const props = defineProps({
  data: Object
})
</script>

<template>
  <div class="currentWeatherComponent">
    <p v-if="props.data.dt" style="min-width: 90px">
      {{ new Date(data.dt).toDateString().slice(0, -7) }}
    </p>
    <p>{{ Math.round(data.temp) }}℃</p>
    <img
      v-if="data.weather[0].icon"
      :src="'https://openweathermap.org/img/wn/' + data.weather[0].icon + '@2x.png'"
      width="50"
      height="50"
    />
    <p>
      {{ data.weather[0].description }}
    </p>
  </div>
</template>
<style scoped>
.currentWeatherComponent {
  display: flex;
  align-items: center;
  font-size: large;
}
p {
  padding-right: 10px;
}
</style>
