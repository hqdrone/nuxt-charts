<template>
  <div class="chart">
    <div class="chart__canvas mb-4">
      <canvas :style="{ backgroundColor: color }" ref="chart"></canvas>
    </div>
    <div class="chart__options d-flex">
      <div class="chart__type mr-2">
        <b-form-select
          v-model="type"
          :options="[
            {value: 'line', text: 'Line'},
            {value: 'bar', text: 'Bar'},
            {value: 'bubble', text: 'Bubble'},
           ]"
          @change="onChangeType"
        ></b-form-select>
      </div>
      <div class="chart__color">
        <b-form-select
          v-model="color"
          :options="[
            {value: '#FFF', text: 'White'},
            {value: '#fafafa', text: 'Gray'},
           ]"
        ></b-form-select>
      </div>
    </div>

  </div>

</template>

<script>
  import Chart from 'chart.js/auto';

  export default {
    props: ['options'],
    data() {
      return {
        type: '',
        color: '#FFF',
        chart: null
      }
    },
    methods: {
      onChangeType() {
        this.chart.config.type = this.type
        this.chart.update();
      }
    },
    mounted() {
      Chart.defaults.color = '#aaa'
      this.chart = new Chart(this.$refs.chart, {
        type: 'line',

        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          },
        },
        data: this.options.data
      })
      this.type = this.chart.config.type
    }
  }
</script>

<style scoped>
  canvas {
    transition: background-color .4s;
  }
</style>
