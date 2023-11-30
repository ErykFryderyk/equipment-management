<template>
  <div>
    <!-- WYDAWANIE SPRZĘTU -->
    <h2>Wydawanie urządzeń</h2>
    <form @submit.prevent="assignDevices">
      <input type="text" v-model="assignData.userLogin" placeholder="login uzytkownika">
      <input type="text" v-model="assignData.selectedScanner" placeholder="Skaner">
      <input type="text" v-model="assignData.selectedPrinter" placeholder="Drukarka">
      <button type="submit">Przydziel urządzenie</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'AssignDevice',
  data() {
    return {
      assignData: {
        userLogin: '',
        selectedScanner: '',
        selectedPrinter: '',
      }
    }
  },
  methods: {
    assignDevices() {
      this.$emit('assignDevices', this.assignData);
    }
  }
}
</script>

<style lang="scss" scoped></style>