<template>
    <div class="gastos-container">
      <h2>Lista de Gastos</h2>
      <table class="gastos-table">
        <tr v-for="(gasto, index) in gastos" :key="index">
          <td :class="{'estado-pendiente': !gasto.pagado, 'estado-pagado': gasto.pagado}">
            {{ gasto.pagado ? 'Pagado' : 'Pendiente' }}
          </td>
          <td>{{ gasto.concepto }}</td>
          <td class="amount">{{ formatCurrency(gasto.monto) }}</td>
          <td>
            <button @click="handleAction(gasto)">
              {{ gasto.pagado ? 'Modificar' : 'Pagar' }}
            </button>
          </td>
        </tr>
      </table>
  
      <!-- Modal para pagar/modificar -->
      <div v-if="showModal" class="modal">
        <div class="modal-content">
          <h3>Resumen de Gasto</h3>
          <p>Concepto: {{ selectedGasto.concepto }}</p>
          <p>Monto Programado: {{ formatCurrency(selectedGasto.monto) }}</p>
          <label for="montoModificado">Modificar Monto:</label>
          <input type="number" v-model="montoModificado" id="montoModificado" />
          <button @click="confirmAction">Aceptar</button>
          <button @click="closeModal">Cancelar</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'GastosView',
    data() {
      return {
        gastos: [
          { concepto: 'Alquiler', monto: 10000, pagado: false },
          { concepto: 'Luz', monto: 1200, pagado: true },
          { concepto: 'Agua', monto: 800, pagado: false },
          { concepto: 'Internet', monto: 500, pagado: true }
        ],
        showModal: false,
        selectedGasto: null,
        montoModificado: 0
      };
    },
    methods: {
      formatCurrency(value) {
        return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(value);
      },
      handleAction(gasto) {
        this.selectedGasto = gasto;
        this.montoModificado = gasto.monto;
        this.showModal = true;
      },
      confirmAction() {
        if (this.selectedGasto) {
          this.selectedGasto.monto = this.montoModificado;
          this.selectedGasto.pagado = true; // Cambia el estado a pagado
          this.closeModal();
        }
      },
      closeModal() {
        this.showModal = false;
        this.selectedGasto = null;
      }
    }
  };
  </script>
  
  <style scoped>
  .gastos-container {
    margin: 20px;
  }
  
  .gastos-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .gastos-table td {
    padding: 10px 15px;
    border-top: 1px solid #ddd;
    border-bottom: 1px solid #ddd;
  }
  
  .estado-pendiente {
    color: white;
    background-color: red;
    text-align: center;
  }
  
  .estado-pagado {
    color: white;
    background-color: green;
    text-align: center;
  }
  
  .amount {
    text-align: right;
  }
  
  button {
    padding: 5px 10px;
    margin: 5px;
  }
  
  /* Modal Styles */
  .modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .modal-content {
    background-color: white;
    padding: 20px;
    border-radius: 5px;
    max-width: 400px;
    width: 100%;
  }
  
  .modal-content h3 {
    margin-top: 0;
  }
  
  .modal-content button {
    margin-top: 10px;
  }
  </style>
  