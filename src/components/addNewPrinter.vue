<template>
  <div>
    <!-- DODAWANIE DRUKARKI DO BAZY -->
    <h2>Dodawanie Drukarki</h2>
    <form @submit.prevent="addPrinter">
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': nameValid }"
        ></span>
        <p class="valid-text">Nazwa drukarki musi zawierać 8 znaków</p>
      </div>
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': modelValid }"
        ></span>
        <p class="valid-text">Pole "Model" musi być wypełnione</p>
      </div>
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': serialValid }"
        ></span>
        <p class="valid-text">Numer seryjny musi zawierać min. 12 znaków</p>
      </div>

      <div class="form-text-field" style="margin-top: 20px">
        <input type="text" maxlength="8" v-model="printer.printerName" required="" autocomplete="off">
        <label :style="{ color: nameValid ? 'green' : 'gray' }">Nazwa Drukarki</label>
      </div>

      <div class="select-input">
        <label for="model" :style="{ color: modelValid ? 'green' : 'gray' }">Model</label>
        <select id="model" v-model="printer.model">
          <option value="">--brak--</option>
          <option value="QLn620">QLn620</option>
          <option value="QLn220">QLn220</option>
          <option value="ZQ630">ZQ630</option>
        </select>
      </div>

      <div class="form-text-field">
        <input type="text" v-model="printer.serialNumber" required="" autocomplete="off">
        <label :style="{ color: serialValid ? 'green' : 'gray' }">Numer Seryjny</label>
      </div>

      <button type="submit">DODAJ</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'addNewPrinter',
  data() {
    return {
      errorText: 'Źle wypełnione pola w formularzu!',
      successText: 'Nowa drukarka została dodana!',
      printer: {
        printerName: 'KON1L',
        model: '',
        serialNumber: ''
      },
      nameValid: false,
      modelValid: false,
      serialValid: false,
    }
  },
  watch: {
    'printer.printerName': function (newVal) {
      this.nameValid = newVal.length === 8;
    },
    'printer.model': function (newVal) {
      this.modelValid = newVal.lenght !== 0 && newVal !== "";
    },
    'printer.serialNumber': function (newVal) {
      this.serialValid = newVal.length >= 12;
    },
  },
  methods: {
    addPrinter() {
      // Walidacja przed zwróceniem urządzeń
      if(this.nameValid && this.modelValid && this.serialValid) {
        this.printer.printerName = this.printer.printerName.toUpperCase();
        this.$emit('addNewPrinter', this.printer);
      } else{
        this.$emit('alertEvent', this.errorText);
        return;
      }
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
.valid-text{
  color: #858585;
  font-size: 14px;
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

// SELECT INPUT
.select-input{
  width: 100%;
  position: relative;
  margin-bottom: 25px;
}
.select-input select {
  font-size: 100%;
  padding: 0.5em;
  outline: none;
  border: 2px solid rgb(200, 200, 200);
  background-color: transparent;
  border-radius: 10px;
  width: 100%;
}
.select-input label{
  font-size: 100%;
  position: absolute;
  left: 0;
  padding: 0.6em;
  margin-left: 0.5em;
  pointer-events: none;
  transition: all 0.3s ease;
  transform: translateY(-60%) scale(.9);
  color: rgb(100, 100, 100);
  margin-left: 0.8em;
  padding: 0.4em;
  background-color: #fff;
}
.select-input :is(input:focus, input:valid)~label {
  transform: translateY(-60%) scale(.9);
  margin: 0em;
  margin-left: 0.8em;
  padding: 0.4em;
  background-color: #fff;
}

.select-input :is(select:focus, select:valid) {
  border-color: #686868;
}
.error {
  color: red;
  font-size: 12px;
}

/* Hide the default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.container {
  display: block;
  position: relative;
  cursor: pointer;
  font-size: 1.5rem;
  user-select: none;
}

/* Create a custom checkbox */
.checkmark {
  --clr: #0B6E4F;
  position: relative;
  top: 0;
  left: 0;
  height: 1.3em;
  width: 1.3em;
  background-color: #ccc;
  border-radius: 50%;
  transition: 300ms;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: var(--clr);
  border-radius: .5rem;
  animation: pulse 500ms ease-in-out;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 0.45em;
  top: 0.25em;
  width: 0.25em;
  height: 0.5em;
  border: solid #E0E0E2;
  border-width: 0 0.15em 0.15em 0;
  transform: rotate(45deg);
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 #0B6E4F90;
    rotate: 20deg;
  }

  50% {
    rotate: -20deg;
  }

  75% {
    box-shadow: 0 0 0 10px #0B6E4F60;
  }

  100% {
    box-shadow: 0 0 0 13px #0B6E4F30;
    rotate: 0;
  }
}
</style>