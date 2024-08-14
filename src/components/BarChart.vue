<template>
  <div>
    <h3>{{ title }}</h3>
    <BarChartComponent :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Bar } from 'vue-chartjs';
import { Chart as ChartJS, BarElement, CategoryScale, LinearScale, Tooltip, Legend } from 'chart.js';

ChartJS.register(BarElement, CategoryScale, LinearScale, Tooltip, Legend);

export default {
  name: 'BarChart',
  components: {
    BarChartComponent: Bar
  },
  props: {
    title: {
      type: String,
      default: 'Ahorros vs Gastos Mensuales'
    },
    data: {
      type: Object,
      required: true
    }
  },
  computed: {
    chartData() {
      const months = Object.keys(this.data).map(month => month.slice(0, 3));
      const ahorros = months.map((_, i) => this.data[Object.keys(this.data)[i]].ahorro);
      const gastos = months.map((_, i) => this.data[Object.keys(this.data)[i]].gastos);

      return {
        labels: months,
        datasets: [
          
        {
            label: 'Gastos',
            backgroundColor: '#d4463c',
            data: gastos
          },{
            label: 'Ahorro',
            backgroundColor: '#8fb930',
            data: ahorros
          }
         
        ]
      };
    },
    chartOptions() {
      return {
        responsive: true,
        scales: {
          x: {
            stacked: true, // Apila las barras en el eje X
            grid: {
              display: false
            }
          },
          y: {
            stacked: true, // Apila las barras en el eje Y
            beginAtZero: true,
            ticks: {
              display: false, // Oculta los valores del eje Y
              callback: function(value) {
                return new Intl.NumberFormat('es-MX', { 
                  style: 'currency', 
                  currency: 'MXN', 
                  minimumFractionDigits: 0, 
                  maximumFractionDigits: 0 
                }).format(value / 1000) + 'K'; // Divide por 1000 y redondea a miles (no visible por display: false)
              }
            }
          }
        },
        plugins: {
          legend: {
            display: false // Oculta la leyenda
          },
          datalabels: {
            display: false // Oculta los valores sobre las columnas
          },
          tooltip: {
            enabled: true // Deshabilita los tooltips
          }
        }
      };
    }
  }
};
</script>

<style scoped>
h3 {
  text-align: center;
  margin-bottom: 20px;
}
</style>
