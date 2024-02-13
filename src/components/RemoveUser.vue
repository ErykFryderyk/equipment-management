<template>
  <div>
    <!-- USUWANIE UŻYTKOWNIKA  -->
    <h2>Usuń użytkownika</h2>
    <form @submit.prevent="removeUserByLogin">
      <span v-if="!loginValid" style="color: red;">Login musi mieć dokładnie 6 znaków. (np. JANKOW)</span><br>
      <div>
        <label for="login">Login:</label>
        <input type="text" maxlength="6" id="login" v-model="existingUser" />
      </div>
      <div>
        <label for="password">Hasło:</label>
        <input type="password" id="password" v-model="passwordToRemoveUser"/>
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
</style>