<template>
    <div>
      <h3>Movimientos</h3>
      <ul>
        <li v-for="(movement, index) in movements" :key="index" class="movement-item">
          <div class="date">{{ movement.date }}</div>
          <div class="description">{{ movement.description }}</div>
          <div class="amount">{{ formatCurrency(movement.amount) }}</div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: 'MovementsList',
    props: {
      movements: {
        type: Array,
        required: true
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
  .movement-item {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid #ccc;
  }
  .date {
    width: 20%;
  }
  .description {
    width: 60%;
  }
  .amount {
    width: 20%;
    text-align: right;
  }
  </style>
  