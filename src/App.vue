<template>
  <div id="app">
    <div>
      <b-form-select class="p-md-2 mb-md-2" v-model="filterSeleted" :options="filtersOptions"></b-form-select>
    </div>
    <div class="container-bartchart">
      <BasicBarChart
        title="Parts Sales 2010 x 2020"
        :xaxisCategory="getXaxisCategory"
        :series="getSeries"
        :min="min"
        :max="max"
      />
      <LineChart
        title="Parts Sales 2010 x 2020"
        :xaxisCategory="getXaxisCategory"
        :series="getSeries"
        :min="min"
        :max="max"
      />
    </div>
  </div>
</template>

<script>
import BasicBarChart from './components/BasicBarChart.vue'
import LineChart from './components/LineChart.vue'

export default {
  name: 'App',
  components: {
    BasicBarChart,
    LineChart
  },
  computed: {
    getXaxisCategory: function () {
      var xaxisCategory = {};
      switch (this.filterSeleted) {
        case 1:
          xaxisCategory.categories = this.xaxis.categories.filter((category, index) => {
            if (category % 2 !== 0) { this.removeSales.push(index) }
            return category % 2 !== 0
          })
          break
        case 2:
          xaxisCategory.categories = this.xaxis.categories.filter((category, index) => {
            if (category % 2 === 0) { this.removeSales.push(index) }
            return category % 2 === 0
          })
          break
        default:
          xaxisCategory = this.xaxis
          break
      }
      return xaxisCategory
    },
    getSeries: function () {
      if (this.filterSeleted) {
        let data = this.series[0].data.filter((data, index) => this.removeSales.includes(index))
        return [{...this.series[0], data}]
      } else {
        return this.series
      }
    }    
  },
  watch: {
    filterSeleted: function () {
      this.removeSales = []
    }
  },
  data: function () {
    return {
      filterSeleted: 0,
      filtersOptions: [
        {value: 1, text: 'Only even years'},
        {value: 2, text: 'Only odd years'},
        {value: 0, text: 'All years'}
      ],
      removeSales: [],
      min: 100,
      max: 100000000000000,
      xaxis: { categories: [2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020] },
      series: [
        {
          name: 'Sales',
          data: [1000, 10000, 100000, 1000000, 10000000, 100000000, 1000000000, 10000000000, 100000000000, 1000000000000, 10000000000000]
        }
      ]
    }
  }
}
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

<style scope>
.container-bartchart {
  display: flex;
  justify-content: center;
}
</style>
