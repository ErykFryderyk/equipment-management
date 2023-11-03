<template>
  <div>
    <h1>DEVICES READY TO USE:</h1>
    <!-- <ul>
      <li v-for="user in users" :key="user.userID">
        {{ user.name }} ({{ user.login }})
      </li>
    </ul> -->
  </div>
</template>

<script>
export default {
  name: 'DevicesList',
  data() {
    return {
      props: {
        users: Array,
      }
    }
  }
}
</script>

<style lang="scss" scoped></style>