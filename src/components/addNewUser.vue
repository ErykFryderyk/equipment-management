<template>
  <div>
    <!-- DODAWANIE NOWEGO UŻYTKOWNIKA -->
    <h2>Dodawanie Nowego Pracownika</h2>
    <form @submit.prevent="addUser">
      <span v-if="!loginValid" style="color: red;">Login musi mieć dokładnie 6 znaków. (JANKOW)</span><br>
      <span v-if="!nameValid" style="color: red;">Imię i nazwisko nie mogą być dłuższe niż 40 znaków.</span>
    
      <div class="search" style="margin-top: 20px">
        <input type="text" maxlength="6" v-model="newUser.login" required="" autocomplete="off">
        <label>Login Pracownika</label>
      </div>

      <div class="search">
        <input type="text" maxlength="40" v-model="newUser.name" required="" autocomplete="off">
        <label>Imię i Nazwisko</label>
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
        login: '',
        name: '',
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
        this.newUser.login = '';
        this.newUser.name = '';
        alert('Uzytkoniwk dodany!');
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
