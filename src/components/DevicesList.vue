<template>
  <div>
    <h2>{{ msg }}</h2>
    <ul>
      <li v-for="user in users" :key="user.userID">
        {{ user.name }} ({{ user.login }})
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'DevicesList',
  props: {
    users: Array,
    msg: String,
  }
}
</script>

<style lang="scss" scoped></style>