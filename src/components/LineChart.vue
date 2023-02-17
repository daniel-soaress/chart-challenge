<template>
  <div class="basic-bar-chart">
    <apexchart
      :width="width"
      type="line"
      :options="chartOptions"
      :series="series"
    ></apexchart>
  </div>
</template>

<script>
import VueApexCharts from 'vue-apexcharts'

export default {
  name: 'BasicBarChart',
  components: {
    apexchart: VueApexCharts
  },
  props: {
    chartId: {
      type: String,
      default: 'mc-bar-chart'
    },
    xaxisCategory: {
      type: [Array, Object],
      default: () => []
    },
    series: {
      type: [Array, Object],
      default: () => []
    },
    width: {
      type: Number,
      default: 750
    },
    min: {
      type: Number,
      default: 0
    },
    max: {
      type: Number,
      default: 1000
    },
    title: {
      type: String,
      default: ''
    }
  },
  computed: {
    chartOptions: function () {
      return {
        chart: { id: this.chartId },
        xaxis: this.xaxisCategory,
        yaxis: { 
          min: this.min,
          max: this.max,
          logarithmic: true,
          labels: { formatter: function (val, index) { return val.toLocaleString('pt-PT', { style: 'currency', currency: 'EUR' }) }
        } },
        dataLabels: { enabled: false },
        title: {
          text: this.title,
          floating: true,
          align: 'center',
          style: {
            color: '#444'
          }
        }
      }
    }
  }
}
</script>
