<script setup>
const props = defineProps({
  list: Array
})
</script>

<template>
  <div class="container">
    <h2 v-if="props.list.length !== 0">8-day Forecast</h2>
    <ul>
      <li v-for="dailyData in list">
        <p v-if="dailyData.date">
          {{ dailyData.date.toDateString().slice(0, -5) }}
        </p>
        <div class style="display: flex; align-items: center">
          <img
            :src="'https://openweathermap.org/img/wn/' + dailyData.icon + '@2x.png'"
            width="50"
            height="50"
          />
          <p>{{ Math.round(dailyData.max) }}/{{ Math.round(dailyData.min) }}℃</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  width: 100%;
}
ul {
  padding-left: 0;
}
li {
  width: 55%;
  display: flex;
  list-style: none;
  align-items: center;
  justify-content: space-between;
  border-radius: 4pt;
}

p {
  display: inline-block;
  font-size: large;
  text-align: center;
}

@media (min-width: 1025px) {
  li {
    width: 40%;
  }
}
</style>
