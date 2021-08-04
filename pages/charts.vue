<template>
  <div class="charts">
    <h2>Charts</h2>
    <div class="charts__tools d-flex mb-4">
      <div class="charts__filter">
        <b-form-select
          v-model="filter"
          :options="[
            {value: '', text: 'All'},
            {value: 'temperature', text: 'Temperature'},
            {value: 'light', text: 'Light'},
            {value: 'humidity', text: 'Humidity'},
           ]"
        ></b-form-select>
      </div>
      <div class="charts__sort ml-auto">
        <b-form-select
          v-model="sort"
          :options="[
            {value: 'newFirst', text: 'New First'},
            {value: 'oldFirst', text: 'Old First'}
           ]"
        ></b-form-select>
      </div>
    </div>
    <div class="charts__grid">
      <div class="charts__compare">
        <div class="charts__label">Compare chart</div>
        <div class="charts__chart">

          <canvas ref="compareChart"></canvas>
        </div>
      </div>
      <div class="charts__chart"
           v-for="chart in filterSortCharts"
           :key="chart.id"
      >
        <app-chart
          :options="chart"
          @onCompare="onCompare"
        ></app-chart>
      </div>

    </div>

  </div>
</template>

<script>
  import AppChart from "../components/AppChart";
  import Chart from 'chart.js/auto';

  export default {
    name: "charts",
    data() {
      return {
        filter: '',
        sort: 'newFirst',
        charts: [],
        compareChart: null,
        compareCharts: []
      }
    },
    computed: {
      filterSortCharts() {
        const ths = this
        const sortCharts = this.charts.sort(function (a, b) {
          if (ths.sort === 'newFirst') {
            return Date.parse(b.date) - Date.parse(a.date);
          } else {
            return Date.parse(a.date) - Date.parse(b.date);
          }
        });
        return this.filter ? sortCharts.filter(chart => chart.title === this.filter) : sortCharts
      }
    },
    methods: {
      async getCharts() {
        const chartsRef = this.$fire.database.ref('charts')
        try {
          const snapshot = await chartsRef.once('value')
          this.charts = snapshot.val()
        } catch (e) {
          console.log(e)
        }
      },
      onCompare(chart) {
        if (this.compareCharts.includes(chart.id)) {
          this.compareChart.data.datasets.splice(this.compareCharts.indexOf(chart.id), 1)
          this.compareCharts = this.compareCharts.filter(el => el !== chart.id)
          if (!this.compareCharts.length) {
            this.compareChart.data.labels = []
          }
        } else {
          this.compareCharts.push(chart.id)
          this.compareChart.data.datasets.push(...chart.data.datasets)
          if (this.compareChart.data.labels.length < chart.data.labels.length) {
            this.compareChart.data.labels = chart.data.labels
          }
        }
        this.compareChart.update()
      }
    },
    mounted() {
      this.getCharts()

      this.compareChart = new Chart(this.$refs.compareChart, {
        type: 'line',
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          },
        },
        data: {
          datasets: []
        }
      })
    },
    component: {
      AppChart
    }
  }
</script>

<style scoped>
  .charts__grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 48px;
  }

  @media (max-width: 991.98px) {
    .charts__grid {
      grid-template-columns: 1fr;
    }
  }

  .charts__chart {
    min-width: 0;
  }

  .charts__label {
    text-align: center;
  }
</style>
