<script>
import { defineComponent, ref } from 'vue'
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
      top: 20,
      left: 0,
      right: 20,
      bottom: 0
    })
    const chartOptions = ref({
      plugins: {
        tooltip: {
          position: 'average',
          mode: 'index',
          offset: { x: 0, y: 10 }
        }
      }
    })
    const click = () => {
      console.log(data)
    }

    return { data, margin, direction, chartOptions, click }
  }
})
</script>

<template>
  <!-- <button @click="click"></button> -->
  <h2 v-if="data.length !== 0" style="padding-left: 20px">HourlyForecast</h2>
  <Chart
    :size="{ width: 500, height: 420 }"
    :data="data"
    :margin="margin"
    :direction="direction"
    :options="chartOptions"
  >
    <template #layers>
      <Grid strokeDasharray="2,2" />
      <Line :dataKeys="['time', 'temp']" :lineStyle="{ stroke: '#2ecc71' }" type="monotone" />
    </template>
    <!-- 表示位置がずれる問題があるため、Tooltipはコメントアウト -->
    <!-- <template #widgets>
      <Tooltip borderColor="#2ecc71" />
    </template> -->
  </Chart>
</template>

<style>
Chart {
  color: #2ecc71;
}
</style>
