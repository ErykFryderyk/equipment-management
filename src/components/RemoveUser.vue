<template>
  <div>
    <!-- USUWANIE UŻYTKOWNIKA  -->
    <h2>Usuń użytkownika</h2>
    <form @submit.prevent="removeUserByLogin">
      <span v-if="!loginValid" style="color: red;">Login musi mieć dokładnie 6 znaków. (np. JANKOW)</span><br>
      <span style="color: red;">Login musi mieć dokładnie 6 znaków. (np. JANWOJ)</span>
      <div class="search">
        <input type="text" maxlength="6" id="login" v-model="existingUser" required="" autocomplete="off">
        <label for="name">Login</label>
      </div>
      <div class="search">
        <input type="password" maxlength="6" id="password" v-model="passwordToRemoveUser" required="" autocomplete="off">
        <label for="name">Hasło Admina</label>
      </div>
      <button type="submit">Usuń użytkownika</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'RemoveUser',
  data() {
    return {
      existingUser: '',
      loginValid: true,
      adminPassowrd: '1234',
      passwordToRemoveUser: '1234',
    }
  },
  watch: {
    'existingUser': function (newVal) {
      this.loginValid = newVal.length === 6 && /^[a-zA-Z]+$/.test(newVal);
    },
  },
  methods: {
    removeUserByLogin() {
      if (this.adminPassowrd === this.passwordToRemoveUser) {
        this.$emit("removeUser", this.existingUser);
      } else { 
        alert("Hasło jest niepoprawne!");
      }
    }
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
.doth{
  width: 5px;
  height: 5px;
  background-color: #a8a8a8;
}
</style>