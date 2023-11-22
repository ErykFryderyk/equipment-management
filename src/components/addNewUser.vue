<template>
  <div>
    <!-- DODAWANIE NOWEGO UŻYTKOWNIKA -->
    <h2>Dodaj nowego użytkownika</h2>
      <form @submit.prevent="addUser">
        <div>
          <label for="login">Login:</label>
          <input type="text" id="login" />
        </div>
        <div>
          <label for="name">Imię i nazwisko:</label>
          <input type="text" id="name" />
        </div>
        <button type="submit">Dodaj użytkownika</button>
      </form>
  </div>
</template>

<script>
  export default {
    name: 'addNewUser',
  }
</script>

<style lang="scss" scoped>

</style>