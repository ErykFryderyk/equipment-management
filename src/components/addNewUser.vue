<template>
  <div>
    <!-- DODAWANIE NOWEGO UŻYTKOWNIKA -->
    <h2>Dodawanie Nowego Pracownika</h2>
    <form @submit.prevent="addUser">
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': loginValid }"
        ></span>
        <p class="valid-text">Login musi mieć dokładnie 6 znaków (np. JANKOW)</p>
      </div>
      <div style="display: flex; align-items: center;">
        <span
          class="correct" 
          :class="{ 'correct--active': nameValid }"
          >
        </span>
        <p class="valid-text">Imię i nazwisko nie dłuższe niż 40 znaków</p>
      </div>
      <div class="form-text-field" style="margin-top: 20px">
        <input type="text" maxlength="6" v-model="newUser.login" required="">
        <label :style="{ color: loginValid ? 'green' : 'gray' }">Login Pracownika</label>
      </div>
      <div class="form-text-field">
        <input type="text" maxlength="40" v-model="newUser.name" required="">
        <label :style="{ color: nameValid ? 'green' : 'gray' }">Imię i Nazwisko</label>
      </div>
      <button type="submit">Dodaj użytkownika</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'AddNewUser',
  data() {
    return {
      errorText: 'Źle wypełnione pola w formularzu!',
      successText: 'Nowy uzytkownik został dodany!',
      newUser: {
        login: '',
        name: '',
      },
      loginValid: false,
      nameValid: false,
    };
  },
  watch: {
    'newUser.login': function (newVal) {
      this.loginValid = newVal.length === 6 && /^[a-zA-Z]+$/.test(newVal);
    },
    'newUser.name': function (newVal) {
      this.nameValid = newVal.length <= 50 && /\s/.test(newVal);
    }
  },
  methods: {
    addUser() {
      if (this.loginValid && this.nameValid) {

        this.newUser.login = this.newUser.login.toUpperCase();
        // Emisja zdarzenia, przekazujemy newUser do komponentu rodzica
        this.$emit('updateData', this.newUser);
        // this.$emit('successAlertEvent', this.successText)
        this.newUser.login = '';
        this.newUser.name = '';
      } else {
        this.$emit('alertEvent', this.errorText);
      }
    },
  },
}
</script>

<style lang="scss" scoped>
form {
  padding: 20px 80px;
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

.valid-text{
  color: #858585;
  font-size: 14px;
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
</style>
