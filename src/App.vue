<template>
  <div id="app">
    <!-- Barra superior con el título y el mes actual -->
    <header class="app-header">
      <h1>Resumen - {{ currentMonth }}</h1>
    </header>

    <!-- Barra de menú superior -->
    <nav class="menu-bar">
      <ul>
        <li @click="selectedPage = 'IngresosView'" :class="{ active: selectedPage === 'IngresosView' }">Ingresos</li>
        <li @click="selectedPage = 'GastosView'" :class="{ active: selectedPage === 'GastosView' }">Gastos</li>
        <li @click="selectedPage = 'balance'" :class="{ active: selectedPage === 'balance' }">Balance</li>
      </ul>
    </nav>

    <!-- Mostrar contenido según la pestaña seleccionada -->
    <section class="content-section">
      <IngresosView v-if="selectedPage === 'IngresosView'" />
      <GastosView v-if="selectedPage === 'GastosView'" />
      <div v-if="selectedPage === 'balance'">
        <!-- Gráfico de Ahorro vs Gastos -->
        <section class="chart-section">
          <BalanceChart :data="balanceData" title="Ahorro vs Gastos" />
        </section>

        <!-- Gráfico de barras de Ahorros vs Gastos Mensuales -->
        <section class="chart-section">
          <BarChart :data="monthlyData" title="Ahorros vs Gastos Mensuales" />
        </section>

        <!-- Tabla con el resumen de gastos y saldo actual -->
        <section class="summary-section">
          <table class="summary-table">
            <tr>
              <td><img src="gastos-icon.png" alt="Gastos" class="icon"> Gastos</td>
              <td class="amount">{{ formatCurrency(balanceData.gastos) }}</td>
            </tr>
            <tr>
              <td><img src="ahorro-icon.png" alt="Ahorro" class="icon"> Ahorro</td>
              <td class="amount">{{ formatCurrency(balanceData.ahorro) }}</td>
            </tr>
            <tr>
              <td><img src="pendiente-icon.png" alt="Pendiente" class="icon"> Pendiente</td>
              <td class="amount">{{ formatCurrency(balanceData.pendiente) }}</td>
            </tr>
            <tr>
              <td><img src="saldo-icon.jpg" alt="Saldo" class="icon"> Saldo Disponible</td>
              <td class="amount">{{ formatCurrency(balanceData.ahorro - balanceData.gastos) }}</td>
            </tr>
          </table>
        </section>
      </div>
    </section>

    <!-- Menú inferior con iconos -->
    <nav class="bottom-menu">
      <ul>
        <li @click="selectedPage = 'IngresosView'"><img src="ingresos-icon.png" alt="Ingresos"></li>
        <li @click="selectedPage = 'GastosView'"><img src="gastos-icon.png" alt="Gastos"></li>
        <li @click="selectedPage = 'balance'"><img src="balance-icon.png" alt="Balance"></li>
      </ul>
    </nav>
  </div>
</template>

<script>
import IngresosView from './components/IngresosView.vue';
import GastosView from './components/GastosView.vue';
import BalanceChart from './components/BalanceChart.vue';
import BarChart from './components/BarChart.vue';

export default {
  name: 'App',
  components: {
    IngresosView,
    GastosView,
    BalanceChart,
    BarChart
  },
  data() {
    return {
      currentMonth: new Date().toLocaleString('default', { month: 'long', year: 'numeric' }),
      selectedPage: 'balance', // Pestaña seleccionada por defecto
      balanceData: {
        gastos: 10747.53,
        ahorro: 21247.00,
        pendiente: 5000.00 // Valor pendiente
      },
      monthlyData: {
        Enero: { ahorro: 15000, gastos: 12000 },
        Febrero: { ahorro: 10000, gastos: 14000 },
        Marzo: { ahorro: 17000, gastos: 11000 },
        Abril: { ahorro: 13000, gastos: 9000 },
        Mayo: { ahorro: 19000, gastos: 8000 },
        Junio: { ahorro: 16000, gastos: 13000 },
        Julio: { ahorro: 20000, gastos: 15000 },
        Agosto: { ahorro: 21000, gastos: 18000 }
      }
    };
  },
  methods: {
    formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(value);
    }
  }
};
</script>

<style>
/* Estilos generales */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}

/* Barra superior */
.app-header {
  background-color: #007bff;
  color: white;
  padding: 15px;
}

/* Barra de menú superior */
.menu-bar ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  justify-content: space-around;
  background-color: #e9ecef;
  margin: 0;
}

.menu-bar ul li {
  cursor: pointer;
  padding: 15px;
}

.menu-bar ul li:hover,
.menu-bar ul li.active {
  cursor: pointer;
  background-color: #007bff;
  color: white;
}

/* Sección del gráfico */
.chart-section {
  margin: 20px 0;
}

.chart-section canvas {
  max-width: 80%; /* Aumenta el tamaño del gráfico */
  margin: auto;
}

/* Tabla de resumen */
.summary-section {
  margin: 20px 10px;
}

.summary-table {
  width: 100%;
  border-collapse: collapse;
}

.summary-table td {
  padding: 10px 15px;
  border-top: 1px solid #ddd;
  border-bottom: 1px solid #ddd;
}

.summary-table .icon {
  width: 24px;
  height: 24px;
  margin-right: 10px;
  vertical-align: middle;
}

.summary-table .amount {
  text-align: right;
}

/* Menú inferior */
.bottom-menu ul {
  list-style-type: none;
  padding: 0;
  display: flex;
  justify-content: space-around;
  background-color: #e9ecef;
  position: fixed;
  bottom: 0;
  width: 100%;
  margin: 0;
}

.bottom-menu ul li img {
  width: 30px;
  height: 30px;
  margin: 10px 0;
}
</style>
