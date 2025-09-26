<!-- BarChart.vue -->
<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import { Chart, registerables } from 'chart.js'

Chart.register(...registerables)

const props = defineProps<{
  data: any
  title?: string
  type?: 'bar' | 'line' | 'pie'
}>()

const chartRef = ref<HTMLCanvasElement>()
let chart: Chart | null = null

const initChart = () => {
  if (!chartRef.value || !props.data) return

  if (chart) {
    chart.destroy()
  }

  chart = new Chart(chartRef.value, {
    type: props.type || 'bar',
    data: props.data,
    options: {
      responsive: true,
      plugins: {
        legend: {
          position: 'top',
        },
        title: {
          display: true,
          text: props.title || 'Chart',
        },
      },
      scales: {
        y: {
          beginAtZero: true,
        },
      },
    },
  })
}

watch(() => props.data, initChart, { deep: true })
onMounted(initChart)
</script>

<template>
  <div class="chart-container">
    <canvas ref="chartRef"></canvas>
  </div>
</template>

<style scoped>
.chart-container {
  position: relative;
  height: 400px;
  width: 100%;
}
</style>
