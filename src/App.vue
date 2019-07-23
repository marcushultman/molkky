<template>
  <div id="app">
    <UsersInput :users.sync="users"/>
    <hr>
    <UserCounter v-for="(user, index) in users" :user="user" :key="index"/>
  </div>
</template>

<script>
import UsersInput from './components/UsersInput.vue'
import UserCounter from './components/UserCounter.vue'

export default {
  name: 'app',
  components: {
    UsersInput,
    UserCounter,
  },
  data() {
    return {
      users: [],
    };
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
