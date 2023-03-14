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
  width: 40%;
}
ul {
  padding-left: 0;
}
li {
  display: flex;
  list-style: none;
  align-items: center;
  border-radius: 4pt;
  justify-content: space-between;
}

p {
  display: inline-block;
  font-size: large;
  text-align: center;
  padding-right: 15px;
  min-width: 120px;
}

@media (min-width: 1025px) {
  li {
    width: 50%;
  }
}
</style>
