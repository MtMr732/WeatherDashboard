<script>
import { defineComponent, ref, watch } from 'vue'
import { Chart, Grid, Line, Tooltip } from 'vue3-charts'

export default defineComponent({
  name: 'Main',
  components: { Chart, Grid, Line, Tooltip },
  props: {
    datalist: Array
  },
  setup(props) {
    const data = ref(props.datalist)

    const direction = ref('horizontal')
    const margin = ref({
      left: 0,
      top: 20,
      right: 20,
      bottom: 0
    })
    const axis = ref({
      primary: {
        type: 'band',
        tickValues: data
      }
    })
    const click = () => {
      console.log(props.datalist)
    }

    return { data, margin, direction, click, axis }
  }
})
</script>

<template>
  <button @click="click">孫ボタン</button>
  <Chart
    v-if="data"
    :size="{ width: 500, height: 400 }"
    :data="data"
    :margin="margin"
    :direction="direction"
  >
    <template #layers>
      <Grid strokeDasharray="2,2" />
      <Line :dataKeys="['time', 'temp']" type="monotone" />
    </template>
    <template #widgets>
      <Tooltip />
    </template>
  </Chart>
</template>

<style>
/* #app {
  color: #2ecc71;
} */
</style>
