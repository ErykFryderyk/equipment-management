<template>
  <div>
    <!-- DODAWANIE DRUKARKI DO BAZY -->
    <h2>Dodaj drukarkę</h2>
    <form @submit.prevent="addPrinter">
      <span v-if="!nameValid" style="color: red;">Nazwa musi zawierać 8 znaków.</span>
      <span v-if="!modelValid" style="color: red;">Pole "Model" nie moze byc puste</span>
      <span v-if="!serialValid" style="color: red;">Numer seryjny do wypełnienia</span>
      <div>
        <label for="printerName" :style="{ color: nameValid ? 'green' : 'red' }">Printer Name:</label>
        <input type="text" id="printerName" placeholder="KON1L001" v-model="printer.printerName" />
      </div>
      <div>
        <label for="model" :style="{ color: modelValid ? 'green' : 'red' }">Model:</label>
        <select id="model" v-model="printer.model">
          <option value="">--brak--</option>
          <option value="QLn620">QLn620</option>
          <option value="QLn220">QLn220</option>
          <option value="ZQ630">ZQ630</option>
        </select>
      </div>
      <div>
        <label for="printerSerialNumber" :style="{ color: serialValid ? 'green' : 'red' }">Serial Number:</label>
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
      },
      nameValid: false,
      modelValid: false,
      serialNumber: false,
    }
  },
  watch: {
    'printer.printerName': function (newVal) {
      this.nameValid = newVal.length === 8;
    },
    'printer.model': function (newVal) {
      this.modelValid = newVal.lenght !== 0;
    },
    'printer.serialNumber': function (newVal) {
      this.serialValid = newVal.length >= 12;
    },
  },
  methods: {
    addPrinter() {
      // Walidacja przed zwróceniem urządzeń
      if(this.nameValid && this.modelValid && this.serialValid) {
        this.$emit('addNewPrinter', this.printer);
      } else{
        alert("wypełnij poprawnie wszytkie pola!");
        return;
      }
    }
  }
}
</script>

<style lang="scss" scoped>
form {
  padding: 20px 100px;
  display: flex;
  justify-content: center;
  align-items: space-between;
  flex-direction: column;
}

input {
  margin-bottom: 30px;
}

.error {
  color: red;
  font-size: 12px;
}
</style>