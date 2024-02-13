<template>
  <div>
    <!-- DODAWANIE NOWEGO UŻYTKOWNIKA -->
    <h2>Dodaj nowego użytkownika</h2>
    <form @submit.prevent="addUser">
      <span v-if="!loginValid" style="color: red;">Login musi mieć dokładnie 6 znaków. (JANKOW)</span><br>
      <span v-if="!nameValid" style="color: red;">Imię i nazwisko nie mogą być dłuższe niż 50 znaków.</span>
      <div>
        <label for="login">Login:</label>
        <input type="text" id="login" maxlength="6" v-model="newUser.login" :style="{ color: loginValid ? 'green' : 'red' }" />
      </div>
      <div>
        <label for="name">Imię i nazwisko:</label>
        <input type="text" id="name" v-model="newUser.name" :style="{ color: nameValid ? 'green' : 'red' }" />
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
      newUser: {
        login: 'DARDAR',
        name: 'Darek Darecki',
      },
      loginValid: true,
      nameValid: true,
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
      } else {
        alert('Źle wypełnione dane w formularzu!');
      }
    },
  },
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
