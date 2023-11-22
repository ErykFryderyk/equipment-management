<template>
  <div>
    <!-- ZDAWANIE SPRZĘTU -->
    <h2>Zdawanie urządzeń</h2>
    <form @submit.prevent="returnDevices">
      <input type="text" placeholder="login uzytkownika">
      <input type="text" placeholder="Skaner">
      <input type="text" placeholder="Drukarka">
      <button type="submit">Przydziel urządzenie</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'ReturnDevice'
}
</script>

<style lang="scss" scoped></style>