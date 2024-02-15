<template>
  <div>
    <h2>Dodaj skaner</h2>
    <form @submit.prevent="addScanner">
      <span v-if="!nameValid" style="color: red;">Nazwa musi zawierać 8 znaków.</span>
      <span v-if="!modelValid" style="color: red;">Pole "Model" nie moze byc puste</span>
      <span v-if="!serialValid" style="color: red;">Numer seryjny do wypełnienia</span>
      <div>
        <label for="scannerName" :style="{ color: nameValid ? 'green' : 'red' }">Scanner Name:</label>
        <input type="text" id="scannerName" maxlength="10" placeholder="KON1S001" v-model="scanner.scannerName"/>
      </div>
      <div>
        <label for="model" :style="{ color: modelValid ? 'green' : 'red' }">Model:</label>
        <select id="model" v-model="scanner.model">
          <option value>--brak--</option>
          <option value="TC52">TC52</option>
          <option value="MC33">MC33</option>
          <option value="MC55">MC55</option>
          <option value="MC67">MC67</option>
        </select>
      </div>
      <div>
        <label for="scannerSerialNumber" :style="{ color: serialValid ? 'green' : 'red' }">Serial Number:</label>
        <input type="text" id="scannerSerialNumber" placeholder="S7265638782641" v-model="scanner.serialNumber" />
      </div>
      <button type="submit">Dodaj skaner</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'addScanner',
  data() {
    return {
      scanner: {
        scannerName: 'KON1S',
        model: '',
        serialNumber: 'S123123',
      },
      nameValid: false,
      modelValid: false,
      serialNumber: false,
    }
  },
  watch: {
    'scanner.scannerName': function (newVal) {
      this.nameValid = newVal.length === 8;
    },
    'scanner.model': function (newVal) {
      this.modelValid = newVal.lenght != '';
    },
    'scanner.serialNumber': function (newVal) {
      this.serialValid = newVal.length >= 12;
    },
  },
  methods: {
    addScanner() {
      // Walidacja przed zwróceniem urządzeń
      if(this.nameValid && this.modelValid && this.serialValid) {
        this.$emit('addNewScanner', this.scanner);
      } else{
        alert("wypełnij poprawnie pola");
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