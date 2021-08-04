<template>
  <div class="chart">
    <div class="chart__compare">
      <b-form-checkbox
        v-model="compareStatus"
        @change="onCompare"
      >
        Compare
      </b-form-checkbox>
    </div>
    <div class="chart__canvas">
      <canvas :style="{ backgroundColor: backgroundColor }" ref="chart"></canvas>
    </div>
    <div class="chart__options d-flex">
      <div class="chart__select mr-2">
        <div class="chart__label">Chart Type</div>
        <div class="chart__value">
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
      </div>
      <div class="chart__select mr-2">
        <div class="chart__label">Chart Color</div>
        <div class="chart__value">
          <b-form-select
            v-model="chartColor"
            :options="[
              {value: '#EB9486', text: 'Red'},
              {value: '#F3DE8A', text: 'Yellow'},
              {value: '#91C4F2', text: 'Blue'}
             ]"
            @change="onChangeColor"
          ></b-form-select>
        </div>
      </div>
      <div class="chart__select">
        <div class="chart__label">Background Color</div>
        <div class="chart__value">
          <b-form-select
            v-model="backgroundColor"
            :options="[
              {value: '#FFF', text: 'White'},
              {value: '#666', text: 'Gray'},
             ]"
          ></b-form-select>
        </div>
      </div>
      <div class="chart__date ml-auto">
        <div class="chart__label">Date</div>
        <div class="chart__value">
          {{options.date}}
        </div>
      </div>
    </div>

  </div>

</template>

<script>
  import Chart from 'chart.js/auto';

  export default {
    emits: ['onCompare'],
    props: ['options'],
    data() {
      return {
        type: '',
        backgroundColor: '#FFF',
        chartColor: '',
        chart: null,
        compareStatus: false
      }
    },
    methods: {
      onChangeType() {
        this.chart.config.type = this.type
        this.chart.update();
      },
      onChangeColor() {
        this.chart.data.datasets[0].borderColor = this.chartColor
        this.chart.data.datasets[0].backgroundColor = this.chartColor
        this.chart.update();
      },
      onCompare() {
        this.$emit('onCompare', this.chart)
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
      this.chartColor = this.chart.data.datasets[0].borderColor
    }
  }
</script>

<style scoped>
  canvas {
    transition: background-color .4s;
  }

  .chart__label {
    font-size: 12px;
    color: #aaa;
  }

  .chart__compare {
    display: flex;
    justify-content: flex-end;
  }
</style>
