<template>
  <div class="wrapper">
    <div class="chart">
      <canvas id="myChart" class="myChart" height="400"></canvas>
    </div>
  </div>
</template>

<script setup>
import Chart from 'chart.js/auto';
import {onMounted} from "vue";

const props = defineProps({
  weatherData: [Array, Object],
})


let hourlyTemp = props.weatherData.hourly.map((hour) => {
  return hour.temp
})

let hourlyTime = props.weatherData.hourly.map((hour) => {
  let time = new Date(hour.currentTime
  ).toLocaleTimeString("en-us", {
    hour: "numeric",
  })
  return time
})


onMounted(() => {
  const myChart = new Chart(document.getElementById('myChart'), {
    type: 'line',
    data: {
      labels: hourlyTime.splice(0, 24),
      datasets: [{
        label: '# of Votes',
        data: hourlyTemp.splice(0, 24),
        borderWidth: 1,
        backgroundColor: "#fff",
      }]
    },
    options: {
      scales: {
        y: {
          beginAtZero: true
        }
      }
    },
  });
})

Chart.defaults.color = '#fff';


</script>

<style scoped lang="scss">
.wrapper {
  height: auto;
  width: 100%;

  .myChart {
    width: 100%;
  }
}
</style>
