<template>
  <div>
    <h2>Dodaj skaner</h2>
    <form @submit.prevent="addScanner">
      <div>
        <label for="scannerName">Scanner Name:</label>
        <input type="text" id="scannerName" maxlength="8" placeholder="KON1S001" v-model="scanner.scannerName" />
      </div>
      <div>
        <label for="model">Model:</label>
        <select id="model" v-model="scanner.model">
          <option value="">--brak--</option>
          <option value="TC52">TC52</option>
          <option value="MC33">MC33</option>
          <option value="MC55">MC55</option>
          <option value="MC67">MC67</option>
        </select>
      </div>
      <div>
        <label for="scannerSerialNumber">Serial Number:</label>
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

      }
    }
  },
  methods: {
    addScanner() {
      this.$emit('addNewScanner', this.scanner);
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