<template>
  <div>
    <h3>{{ title }}</h3>
    <DoughnutChart :data="chartData" :options="chartOptions" />
  </div>
</template>

<script>
import { Doughnut } from 'vue-chartjs';
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js';
import ChartDataLabels from 'chartjs-plugin-datalabels';

ChartJS.register(ArcElement, Tooltip, Legend, ChartDataLabels);

export default {
  name: 'BalanceChart',
  components: {
    DoughnutChart: Doughnut
  },
  props: {
    title: {
      type: String,
      default: 'Balance General'
    },
    data: {
      type: Object,
      required: true
    }
  },
  computed: {
    chartData() {
      return {
        labels: ['Gastos', 'Ahorro', 'Pendiente'],
        datasets: [
          {
            backgroundColor: ['#d4463c', '#8fb930', '#FFCE56'],
            data: [this.data.gastos, this.data.ahorro, this.data.pendiente]
          }
        ]
      };
    },
    chartOptions() {
      return {
        cutout: '50%',
        plugins: {
          datalabels: {
            color: 'white',
            formatter: (value) => {
              const total = this.data.gastos + this.data.ahorro + this.data.pendiente;
              const percentage = ((value / total) * 100).toFixed(1) + '%';
              return `${this.formatCurrency(value)}\n(${percentage})`;
            },
            font: {
              weight: 'bold',
              size: 12
            }
          },
          tooltip: {
            callbacks: {
              label: function (tooltipItem) {
                let label = tooltipItem.label || '';
                if (label) {
                  label += ': ';
                }
                label += new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(tooltipItem.raw);
                return label;
              }
            }
          }
        }
      };
    }
  },
  methods: {
    formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(value);
    }
  }
};
</script>

<style scoped>
h3 {
  text-align: center;
  margin-bottom: 20px;
}
/* Ajusta el tamaño del canvas del gráfico */
.chart-section canvas {
  max-width: 100%; /* Incrementa el tamaño del gráfico al 80% del contenedor */
  margin: auto;
}
</style>
