<script setup>
import { ref } from 'vue'

const lat = ref()
const lon = ref()
const clickSubmit = () => {
  console.log('lat:' + lat.value, 'lon:' + lon.value)
}
</script>

<template>
  <input v-model="lat" type="text" placeholder="経度:lat" />
  <input v-model="lon" type="text" placeholder="緯度:lon" />
  <button @click="clickSubmit">更新</button>
</template>

<style></style>
