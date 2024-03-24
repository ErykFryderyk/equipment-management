<template>
  <div>
    <h2>Dodawanie Skanera</h2>
    <form @submit.prevent="addScanner">
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': nameValid }"
        ></span>
        <p class="valid-text">Nazwa skanera musi zawierać 8 znaków</p>
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
        <input type="text" maxlength="8" v-model="scanner.scannerName" required="" autocomplete="off">
        <label :style="{ color: nameValid ? 'green' : 'gray' }">Nazwa Skanera</label>
      </div>

      <div class="select-input">
        <label for="model" :style="{ color: modelValid ? 'green' : 'gray' }">Model</label>
        <select id="model" v-model="scanner.model">
          <option value="">--brak--</option>
          <option value="TC52">TC52</option>
          <option value="MC33">MC33</option>
          <option value="MC55">MC55</option>
          <option value="MC67">MC67</option>
        </select>
      </div>

      <div class="form-text-field">
        <input type="text" v-model="scanner.serialNumber" required="" autocomplete="off">
        <label :style="{ color: serialValid ? 'green' : 'gray' }">Numer Seryjny</label>
      </div>
      <button type="submit">DODAJ</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'addScanner',
  data() {
    return {
      scanner: {
        scannerName: 'KON1S',
        model: '',
        serialNumber: '',
      },
      nameValid: false,
      modelValid: false,
      serialValid: false,
    }
  },
  watch: {
    'scanner.scannerName': function (newVal) {
      this.nameValid = newVal.length === 8;
    },
    'scanner.model': function (newVal) {
      this.modelValid = newVal.lenght !== 0 && newVal !== "";
    },
    'scanner.serialNumber': function (newVal) {
      this.serialValid = newVal.length >= 12;
    },
  },
  methods: {
    addScanner() {
      // Walidacja przed zwróceniem urządzeń
      if(this.nameValid && this.modelValid && this.serialValid) {
        this.$emit('addNewScanner', this.scanner);
      } else{
        alert("wypełnij poprawnie pola");
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

.error {
  color: red;
  font-size: 12px;
}

// FORM TEXT INPUT
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
</style>