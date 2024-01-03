<template>
  <div>
    <!-- ZDAWANIE SPRZĘTU -->
    <h2>Zdawanie urządzeń</h2>
    <form @submit.prevent="returnDevices">
      <input type="text" v-model="userLogin" placeholder="G">
      <!-- <input type="text" v-model="formData.selectedScanner" placeholder="Skaner"> -->
      <!-- <input type="text" v-model="formData.selectedPrinter" placeholder="Drukarka"> -->
      <button type="submit">Zwróć urządzenia</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'ReturnDevice',
  data() {
    return {
      userLogin: 'G',
    }
  },
  methods: {
    returnDevices() {
      console.log(this.userLogin);
      this.$emit('returnDevices', this.userLogin);
    }
  }
}
</script>

<style lang="scss" scoped></style>