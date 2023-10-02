<script setup>
import {computed, ref} from "vue";

const props = defineProps({
  propety: [Object]
})


const seriesq = computed(() => {
  const income = props.propety.map((el) => el.profit.toFixed(2));
  const incomePersent = props.propety.map((el) => el.profitPercent.toFixed(2));
  const categories = props.propety.map((el) => el.month);

  const data = {

    series: [{
      name: 'Доход',
      type: 'column',
      data: income
    }, {
      name: 'Проценты',
      type: 'line',
      data: incomePersent
    }],
    chartOptions: {
      chart: {
        height: 350,
        type: 'line',
      },
      stroke: {
        width: [0, 4]
      },
      dataLabels: {
        enabled: true,
        enabledOnSeries: [1]
      },
      labels: categories,
      xaxis: {
        type: 'datetime'
      },
      yaxis: [{
        axisTicks: {
          show: true,
        },
        axisBorder: {
          show: true,
          color: '#008FFB'
        },
        labels: {
          style: {
            colors: '#008FFB',
          }
        }
      }, {
        seriesName: 'Income',
        opposite: true,
        axisTicks: {
          show: true,
        },
        axisBorder: {
          show: true,
          color: '#00E396'
        },
        labels: {
          style: {
            colors: '#00E396',
          }
        },
      }]
    },
  }
  return data
})

</script>

<template>
  <div id="chart">
    <apexchart width="500" type="line" :options="seriesq.chartOptions" :series="seriesq.series"></apexchart>
  </div>
</template>

<style scoped>

</style>