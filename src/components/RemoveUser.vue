<template>
  <div>
    <!-- USUWANIE UŻYTKOWNIKA  -->
    <h2>Usuń użytkownika</h2>
    <form @submit.prevent="removeUserByLogin">
      <div>
        <label for="login">Login:</label>
        <input type="text" id="login" v-model="existingUser" />
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
    }
  },
  methods: {
    removeUserByLogin() {
      this.$emit("removeUser", this.existingUser);
    }
  },
}
</script>

<style lang="scss" scoped></style>