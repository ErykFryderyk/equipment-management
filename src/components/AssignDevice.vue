<template>
  <div>
    <!-- WYDAWANIE SPRZĘTU -->
    <h2>Wydawanie urządzeń</h2>
    <form @submit.prevent="assignDevices">
      <!-- error messages -->
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': userValid }"
        ></span>
        <p class="valid-text">Login musi zawierać 6 znaków</p>
      </div>
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': scannerValid }"
        ></span>
        <p class="valid-text">Nazwa skanera musi zawierać "KON1S***"</p>
      </div>
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': printerValid }"
        ></span>
        <p class="valid-text">Nazwa drukarki musi zawierać "KON1L***"</p>
      </div>

      <div class="form-text-field" style="margin-top: 20px;">
        <input type="text" required="" autocomplete="off" maxlength="6" v-model="formData.user">
        <label for="name" :style="{color: userValid ? 'green' : 'gray'}">Login Pracownika</label>
      </div>
      <div class="form-text-field">
        <input type="text" required="" autocomplete="off" maxlength="8" v-model="formData.scanner">
        <label for="name" :style="{color: scannerValid ? 'green' : 'gray'}">Nazwa Skanera</label>
      </div>
      <div class="form-text-field">
        <input type="text" required="" autocomplete="off" maxlength="8" v-model="formData.printer">
        <label for="name" :style="{color: printerValid ? 'green' : 'gray'}">Nazwa Drukarki</label>
      </div>
      <button type="submit">Przydziel urządzenie</button>
    </form>
    <!-- <div class="timer"> -->
    <!-- <ProgressBar/> -->
    <!-- </div> -->
  </div>
</template>

<script>
export default {
  name: 'AssignDevice',
  data() {
    return {
      errorText: 'Źle wypełnione pola w formularzu!',
      formData: {
        user: '',
        scanner: '',
        printer: '',
      },
      userValid: false,
      scannerValid: false,
      printerValid: false,
    }
  },
  watch: {
    'formData.user': function (newVal) {
      this.userValid = newVal.length === 6 && /^[a-zA-Z]+$/.test(newVal);
    },
    'formData.scanner': function (newVal) {
      this.scannerValid = newVal.length === 8 && newVal.toUpperCase().includes('KON1S');
    },
    'formData.printer': function (newVal) {
      this.printerValid = newVal.length === 8 && newVal.toUpperCase().includes('KON1L');
    },
  },
  methods: {
    assignDevices() {
      
      if (this.userValid && this.scannerValid && this.printerValid) {
        this.formData.user = this.formData.user.toUpperCase();
        this.formData.scanner = this.formData.scanner.toUpperCase();
        this.formData.printer = this.formData.printer.toUpperCase();
        this.$emit('assignDevices', this.formData);
        // this.$emit('successAlertEvent', this.successText);
      } else {
        this.$emit('alertEvent', this.errorText);
        return;
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
.valid-text{
  color: #858585;
  font-size: 14px;
}
.correct{
  width: 10px;
  height: 10px;
  margin-right: 5px;
  border-radius: 50px;
  border: 1px solid gray;
  background-color: transparent;
  transition: background-color .3s ease-in-out, border .3s ease-in-out;

  &--active{
    position: relative;
    border: 1px solid gray;
    background-color: #63bd63;

    &::after{
      position: absolute;
      content: '';
      width: 10px;
      height: 10px;
      border: 1px solid green;
      border-radius: 50px;
      background-color: transparent;
      transform: translate(-50%, -50%);
      left: 50%;
      top:50%;
      animation: dissolve 0.4s ease forwards;
    }
  }

  @keyframes dissolve {
    0% {
      opacity: 100%;
    }
    100% {
      opacity: 0;
      transform: translate(-50%, -50%) scale(3);
    }
  }
}

input {
  margin-bottom: 30px;
}

// FORM INPUT
.form-text-field {
  width: 100%;
  position: relative;
}

.form-text-field input {
  font-size: 100%;
  padding: 0.5em;
  outline: none;
  border: 2px solid rgb(200, 200, 200);
  background-color: transparent;
  border-radius: 10px;
  width: 100%;
}

.form-text-field label {
  font-size: 100%;
  position: absolute;
  left: 0;
  padding: 0.6em;
  margin-left: 0.5em;
  pointer-events: none;
  transition: all 0.3s ease;
  color: rgb(100, 100, 100);
}

.form-text-field :is(input:focus, input:valid)~label {
  transform: translateY(-60%) scale(.9);
  margin: 0em;
  margin-left: 0.8em;
  padding: 0.4em;
  background-color: #fff;
}

.form-text-field :is(input:focus, input:valid) {
  border-color: #686868;
}

.error {
  color: red;
  font-size: 12px;
}
</style>