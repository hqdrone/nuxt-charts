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
      <div class="charts__chart"
           v-for="chart in filterSortCharts"
           :key="chart.id"
      >
        <app-chart
          :options="chart"
        ></app-chart>
      </div>

    </div>

  </div>
</template>

<script>
  import AppChart from "../components/AppChart";

  export default {
    name: "charts",
    data() {
      return {
        filter: '',
        sort: 'newFirst',
        charts: []
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
          console.log(this.charts)
        } catch (e) {
          console.log(e)
        }
      }
    },
    mounted() {
      this.getCharts()
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
</style>
