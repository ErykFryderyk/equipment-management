<template>
  <div>
    <!-- DODAWANIE DRUKARKI DO BAZY -->
    <h2>Dodaj drukarkę</h2>
    <form @submit.prevent="addPrinter">
      <div>
        <label for="printerName">Printer Name:</label>
        <input type="text" id="printerName" placeholder="KON1L001" />
      </div>
      <div>
        <label for="model">Model:</label>
        <select id="model">
          <option value="">--brak--</option>
          <option value="QLn620">QLn620</option>
          <option value="QLn220">QLn220</option>
          <option value="ZQ630">ZQ630</option>
        </select>
      </div>
      <div>
        <label for="printerSerialNumber">Serial Number:</label>
        <input type="text" id="printerSerialNumber" placeholder="S1203120312031" />
      </div>
      <button type="submit">Dodaj skaner</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'addNewPrinter',
}
</script>

<style lang="scss" scoped></style>