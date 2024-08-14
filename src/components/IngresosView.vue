<template>
  <div>
    <h2>Lista de Ingresos</h2>
    <ul>
      <li v-for="(ingreso, index) in ingresos" :key="index">
        {{ ingreso.concepto }} - {{ formatCurrency(ingreso.monto) }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'IngresosView',
  data() {
    return {
      ingresos: [
        { concepto: 'Salario', monto: 20000 },
        { concepto: 'Freelance', monto: 5000 }
      ]
    };
  },
  methods: {
    formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(value);
    }
  }
};
</script>

<style scoped>
h2 {
  text-align: center;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  padding: 10px;
  border-bottom: 1px solid #ddd;
}
</style>
