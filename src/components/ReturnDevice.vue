<template>
  <div>
    <!-- ZDAWANIE SPRZĘTU -->
    <h2>Zdawanie urządzeń</h2>
    <form @submit.prevent="returnDevices">
      <!-- error messages -->
      <span v-if="userLoginError" class="error">Login musi mieć co najmniej 6 znaków.</span>
      <span v-if="userLoginEmptyError" class="error">Login nie może być pusty.</span>
      <span v-if="scannerNameError" class="error">Nazwa skanera musi zawierać "KON1S".</span>
      <span v-if="scannerNameEmptyError" class="error">Nazwa skanera nie może być pusta.</span>
      <span v-if="printerNameError" class="error">Nazwa drukarki musi zawierać "KON1L".</span>
      <span v-if="printerNameEmptyError" class="error">Nazwa drukarki nie może być pusta.</span>

      <input type="text" maxlength="6" v-model="formData.userLogin" placeholder="Login pracownika">
      <input type="text" maxlength="8" v-model="formData.selectedScanner" placeholder="Nazwa skanera">
      <input type="text" maxlength="8" v-model="formData.selectedPrinter" placeholder="Nazwa Drukarka">
      <button type="submit">Zwróć urządzenia</button>
    </form>
    <!-- <div class="timer"> -->
    <!-- <ProgressBar/> -->
    <!-- </div> -->
  </div>
</template>

<script>
import ProgressBar from './ProgressBar.vue';

export default {
  name: 'ReturnDevice',
  components: {
    ProgressBar,
  },
  data() {
    return {
      formData: {
        userLogin: '',
        selectedScanner: '',
        selectedPrinter: '',
      },
      userLoginError: false,
      scannerNameError: false,
      printerNameError: false,
    }
  },
  methods: {
    returnDevices() {
      // this.checkEmptyInput();

      const login = this.formData.userLogin.toUpperCase();
      const scannerName = this.formData.selectedScanner.toUpperCase();
      const printerName = this.formData.selectedPrinter.toUpperCase();

      // Walidacja przed zwróceniem urządzeń
      this.userLoginError = login.length < 5;
      this.userLoginEmptyError = login === ''
      this.scannerNameError = !scannerName.includes('KON1S');
      this.scannerNameEmptyError = scannerName === '';
      this.printerNameError = !printerName.includes('KON1L');
      this.printerNameEmptyError = printerName === '';

      // Walidacja formularza
      // Sprawdzenie, czy wszystkie warunki walidacji są spełnione...
      if (!this.userLoginError && !this.scannerNameError && !this.printerNameError) {
        this.$emit('returnDevices', [login,scannerName,printerName]);
        this.formData.userLogin = '';
        this.formData.selectedPrinter = '';
        this.formData.selectedScanner = '';
      };
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