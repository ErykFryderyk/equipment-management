<template>
  <div>
    <!-- DODAWANIE NOWEGO UŻYTKOWNIKA -->
    <h2>Dodaj nowego użytkownika</h2>
    <form @submit.prevent="addUser">
      <div>
        <label for="login">Login:</label>
        <input type="text" id="login" v-model="newUser.login" />
      </div>
      <div>
        <label for="name">Imię i nazwisko:</label>
        <input type="text" id="name" v-model="newUser.name" />
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
        login: 'Darek',
        name: 'Darek Darecki',
      }
    };
  },
  methods: {
    addUser() {
      // Emisja zdarzenia, przekazujemy newUser do komponentu rodzica
      this.$emit('updateData', this.newUser);
    },
  },
}
</script>

<style lang="scss" scoped></style>