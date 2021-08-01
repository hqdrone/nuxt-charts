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
        charts: [
          {
            id: 1,
            title: 'temperature',
            date: '2021-08-05',
            data: {
              labels: ['June', 'July', 'August'],
              datasets: [{
                label: 'Temperature',
                data: [21, 23, 19],
                backgroundColor: '#EB9486',
                borderColor: '#EB9486',
                borderWidth: 2
              }]
            }
          },
          {
            id: 2,
            title: 'light',
            date: '2021-08-03',
            data: {
              labels: ['June', 'July', 'August'],
              datasets: [{
                label: 'Light',
                data: [45, 120, 60],
                backgroundColor: '#F3DE8A',
                borderColor: '#F3DE8A',
                borderWidth: 2
              }]
            }
          },
          {
            id: 3,
            title: 'humidity',
            date: '2021-08-02',
            data: {
              labels: ['June', 'July', 'August'],
              datasets: [{
                label: 'Humidity',
                data: [46, 13, 24],
                backgroundColor: '#91C4F2',
                borderColor: '#91C4F2',
                borderWidth: 2
              }]
            }
          },
          {
            id: 4,
            title: 'humidity',
            date: '2021-07-31',
            data: {
              labels: ['June', 'July', 'August'],
              datasets: [{
                label: 'Humidity',
                data: [43, 23, 24],
                backgroundColor: '#91C4F2',
                borderColor: '#91C4F2',
                borderWidth: 2
              }]
            }
          },
          {
            id: 5,
            title: 'humidity',
            date: '2021-08-04',
            data: {
              labels: ['June', 'July', 'August'],
              datasets: [{
                label: 'Humidity',
                data: [64, 48, 24],
                backgroundColor: '#91C4F2',
                borderColor: '#91C4F2',
                borderWidth: 2
              }]
            }
          }
        ]
      }
    },
    computed: {
      filterSortCharts() {
        const ths = this
        const sortCharts = this.charts.sort(function(a,b){
          if (ths.sort === 'newFirst') {
            return Date.parse(b.date) - Date.parse(a.date);
          } else {
            return Date.parse(a.date) - Date.parse(b.date);
          }
        });
        return this.filter ? sortCharts.filter(chart => chart.title === this.filter) : sortCharts
      }
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
</style>
