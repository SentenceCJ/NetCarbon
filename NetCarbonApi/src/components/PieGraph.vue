<template>
  <div class="component-container">
    <div class="container">
      <h3 style="text-align: center;">Utilisation des fusées</h3>
      <canvas id="pie-chart" ></canvas>
    </div></div>
  </template>
  
  <script>
  import { Chart, registerables } from 'chart.js';
  Chart.register(...registerables);
  
  export default {
    mounted() {
      this.initChart();
    },
    methods: {
      initChart() {
        const ctx = document.getElementById('pie-chart').getContext('2d');
        const data = {
          labels: ['Falcon 9', 'Falcon Heavy', 'Falcon 1', 'Starship'],
          datasets: [
            {
              data: [95,4, 1, 0],
              backgroundColor: ['#FF6384', '#36A2EB', '#FFCE56', '#3ADF00'],
            },
          ],
        };
        new Chart(ctx, {
          type: 'pie',
          data: data,
        });
      },
    },
  };
  </script>

<style>

.container{
    width: 300px;
}

.component-container {
  background-color: #f2f2f2;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  margin-right: 10px;
}
</style>