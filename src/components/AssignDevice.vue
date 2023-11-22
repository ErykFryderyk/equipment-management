<template>
  <div>
    <!-- WYDAWANIE SPRZĘTU -->
    <h2>Wydawanie urządzeń</h2>
      <form @submit.prevent="assignDevices">
        <input type="text" v-model="userLogin" placeholder="login uzytkownika">
        <input type="text" v-model="selectedScanner" placeholder="Skaner">
        <input type="text" v-model="selectedPrinter" placeholder="Drukarka">
        <button type="submit">Przydziel urządzenie</button>
      </form>
  </div>
</template>

<script>
  export default {
    name: 'AssignDevice',
  }
</script>

<style lang="scss" scoped>

</style>