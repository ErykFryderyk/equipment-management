<template>
  <div>
    <!-- DODAWANIE DRUKARKI DO BAZY -->
    <h2>Dodawanie Drukarki</h2>
    <form @submit.prevent="addPrinter">
      <span v-if="!nameValid" style="color: red;">Nazwa musi zawierać 8 znaków.</span>
      <span v-if="!modelValid" style="color: red;">Pole "Model" nie moze byc puste</span>
      <span v-if="!serialValid" style="color: red;">Numer seryjny do wypełnienia</span>
      <!-- <div> -->
        <!-- <label for="printerName" :style="{ color: nameValid ? 'green' : 'red' }">Printer Name:</label>
        <input type="text" id="printerName" placeholder="KON1L001" v-model="printer.printerName" />
      </div> -->

      <div class="search" style="margin-top: 20px">
        <input type="text" maxlength="8" v-model="printer.printerName" required="" autocomplete="off">
        <label :style="{ color: nameValid ? 'green' : 'gray' }">Nazwa Drukarki</label>
      </div>

      <div>
        <label for="model" :style="{ color: modelValid ? 'green' : 'red' }">Model:</label>
        <select id="model" v-model="printer.model">
          <option value="">--brak--</option>
          <option value="QLn620">QLn620</option>
          <option value="QLn220">QLn220</option>
          <option value="ZQ630">ZQ630</option>
        </select>
      </div>
      <!-- <div>
        <label for="printerSerialNumber" :style="{ color: serialValid ? 'green' : 'red' }">Serial Number:</label>
        <input type="text" id="printerSerialNumber" placeholder="S1203120312031" v-model="printer.serialNumber" />
      </div> -->

      <div class="search">
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
      this.modelValid = newVal.lenght !== 0;
    },
    'printer.serialNumber': function (newVal) {
      this.serialValid = newVal.length >= 12;
    },
  },
  methods: {
    addPrinter() {
      // Walidacja przed zwróceniem urządzeń
      if(this.nameValid && this.modelValid && this.serialValid) {
        this.$emit('addNewPrinter', this.printer);
      } else{
        alert("wypełnij poprawnie wszytkie pola!");
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

input {
  margin-bottom: 30px;
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