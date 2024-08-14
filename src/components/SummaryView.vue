<template>
  <div>
    <h2>Resumen - {{ month }}</h2>
    <BalanceChart :data="balanceData" title="Ahorro vs Gastos" />
    <!-- Aquí puedes agregar un gráfico de barras si lo deseas -->
  </div>
</template>

<script>
import BalanceChart from './BalanceChart.vue';

export default {
  name: 'SummaryView',  // Cambia el nombre aquí
  components: {
    BalanceChart
  },
  props: {
    month: {
      type: String,
      default: 'Mayo 2019'
    },
    balanceData: {
      type: Object,
      required: true
    }
  }
};
</script>

<style scoped>
h2 {
  text-align: center;
  margin-bottom: 20px;
}
</style>
