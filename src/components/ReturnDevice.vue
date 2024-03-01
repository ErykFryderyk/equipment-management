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

      <!-- <input type="text" maxlength="6" v-model="formData.userLogin" placeholder="Login pracownika"> -->
      <div class="search">
        <input type="text" maxlength="6" v-model="formData.userLogin" required="" autocomplete="off">
        <label for="name">Login Pracownika</label>
      </div>
      <!-- <input type="text" maxlength="8" v-model="formData.selectedScanner" placeholder="Nazwa skanera"> -->
      <div class="search">
        <input type="text" maxlength="8" v-model="formData.selectedScanner" required="" autocomplete="off">
        <label for="name">Nazwa Skanera</label>
      </div>
      <!-- <input type="text" maxlength="8" v-model="formData.selectedPrinter" placeholder="Nazwa Drukarka"> -->
      <div class="search">
        <input type="text" maxlength="8" v-model="formData.selectedPrinter" required="" autocomplete="off">
        <label for="name">Nazwa Drukarki</label>
      </div>
      <button type="submit">Usuń Pracownika</button>
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
      userLoginEmptyError: false,
      scannerNameEmptyError: false,
      printerNameEmptyError: false,
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
      };
    },
     clearInputs() {
      this.formData.login = '';
      this.formData.scanner = '';
      this.formData.printer = '';
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