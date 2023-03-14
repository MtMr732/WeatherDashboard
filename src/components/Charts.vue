<script>
import { defineComponent, ref } from 'vue'
import { Chart, Grid, Line, Tooltip } from 'vue3-charts'

export default defineComponent({
  name: 'LineChart',
  components: { Chart, Grid, Line, Tooltip },
  props: {
    datalist: Array
  },
  setup(props) {
    const data = ref(props.datalist)

    const direction = ref('horizontal')
    const margin = ref({
      top: 20,
      left: 0,
      right: 20,
      bottom: 0
    })

    return { data, margin, direction }
  }
})
</script>

<template>
  <Chart :size="{ width: 550, height: 420 }" :data="data" :margin="margin" :direction="direction">
    <template #layers>
      <Grid strokeDasharray="2,2" />
      <Line :dataKeys="['time', 'temp']" :lineStyle="{ stroke: '#EC6E4C' }" type="monotone" />
    </template>
    <template #widgets> <Tooltip /> </template>
  </Chart>
</template>

<style></style>
