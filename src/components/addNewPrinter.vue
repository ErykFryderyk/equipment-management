<template>
  <div>
    <!-- DODAWANIE DRUKARKI DO BAZY -->
    <h2>Dodaj drukarkę</h2>
    <form @submit.prevent="addPrinter">
      <div>
        <label for="printerName">Printer Name:</label>
        <input type="text" id="printerName" placeholder="KON1L001" v-model="printer.printerName" />
      </div>
      <div>
        <label for="model">Model:</label>
        <select id="model" v-model="printer.model">
          <option value="">--brak--</option>
          <option value="QLn620">QLn620</option>
          <option value="QLn220">QLn220</option>
          <option value="ZQ630">ZQ630</option>
        </select>
      </div>
      <div>
        <label for="printerSerialNumber">Serial Number:</label>
        <input type="text" id="printerSerialNumber" placeholder="S1203120312031" v-model="printer.serialNumber" />
      </div>
      <button type="submit">Dodaj skaner</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'addNewPrinter',
  data() {
    return {
      printer: {
        printerName: 'KON1L',
        model: '',
        serialNumber: 'Q123123123'
      }
    }
  },
  methods: {
    addPrinter() {
      this.$emit('addNewPrinter', this.printer);
    }
  }
}
</script>

<style lang="scss" scoped></style>