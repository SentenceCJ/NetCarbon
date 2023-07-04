<template>
    <div class="component-container">
        <h3 style="text-align: center;">Taux d'émission de CO2 </h3>
        <br>
      <canvas ref="chartCanvas"></canvas>
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import { Chart, CategoryScale, LinearScale, BarElement, Title } from 'chart.js';
  
  export default defineComponent({
    mounted() {
      this.renderChart();
    },
    methods: {
      renderChart() {
        Chart.register(CategoryScale, LinearScale, BarElement, Title);
  
        const chartData = {
          labels: ['Falcon 1', 'Falcon 9', 'Falcon Heavy', 'Starship (X)'],
          
          datasets: [
            {
              label: 'CO2 émis approximativement (en Tonne)',
              data: [80, 250, 1500, 0], // donné obtenu en ligne et par calcul regle de 3 pour F1
              backgroundColor: 'rgba(75, 192, 192, 0.8)',
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 1,
            },
          ],
        };
  
        const chartOptions = {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true,
            },
          },
        };
  
        const chartConfig = {
          type: 'bar',
          data: chartData,
          options: chartOptions,
        };
  
        const ctx = this.$refs.chartCanvas.getContext('2d');
        new Chart(ctx, chartConfig);
      },
    },
  });
  </script>
  
  <style scoped>
  canvas {
    max-width: 600px;
    
    
    margin: 0 auto;
    
  }

  .component-container {
  background-color: #f2f2f2;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 20px;
  margin-right: 10px;
}
  </style>
  