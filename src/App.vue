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
body{
  background-color: #d0d8dc;
}
#app {
  font-family: 'Poppins', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #646464;
  margin-left: 15%;
  margin-right: 15%;
  background-color: #fff;
  border-radius: 3px;
}
.center{ 
  text-align: center;
}
.padding-container{
  padding-left: 15px;
  padding-right: 15px;
}
</style>
