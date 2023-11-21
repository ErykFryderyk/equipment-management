<template>
  <div>
    <h2>Dodaj skaner</h2>
    <form @submit.prevent="addScanner">
      <div>
        <label for="scannerName">Scanner Name:</label>
        <input type="text" id="scannerName" maxlength="8" placeholder="KON1S001" />
      </div>
      <div>
        <label for="model">Model:</label>
        <select id="model">
          <option value="">--brak--</option>
          <option value="TC52">TC52</option>
          <option value="MC33">MC33</option>
          <option value="MC55">MC55</option>
          <option value="MC67">MC67</option>
        </select>
      </div>
      <div>
        <label for="scannerSerialNumber">Serial Number:</label>
        <input type="text" id="scannerSerialNumber" placeholder="S7265638782641" />
      </div>
      <button type="submit">Dodaj skaner</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'addScanner',
  methods: {
    addScanner() {
      console.log("Funkcja z komponentu addScanner");
    }
  }
}
</script>

<style lang="scss" scoped></style>