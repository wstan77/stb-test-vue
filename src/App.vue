<template>
  <div id="app">
    <div class="center">
      <img alt="Vue logo" src="./assets/images/users.png">
    </div>
    <AppMenu />
    <Users />
  </div>
</template>

<script>
import Users from './components/Users.vue'
import AppMenu from './components/AppMenu.vue'
 
export default {
  name: 'app',
  components: {
    Users,
    AppMenu
  } 
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #646464;
  margin-left: 15%;
  margin-right: 15%;
}
.center{ 
  text-align: center;
}
</style>
