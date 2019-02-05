<template>
  <ul class="menu-list">
    <li class="active"><a href="/">Usuarios</a></li>
    <li><a>Cŕeditos</a></li>
    <li><a>Contacto</a></li>
  </ul>
</template>
<script>
export default {
  name: "AppMenu"
}
</script>
<style scoped>
  ul.menu-list{
    list-style-type: none;
    margin: 0;
    padding: 0;
    overflow: hidden;
    background-color: #333;
  }
  li{
    float: left;
    border-right: 1px solid #bbb;
  }
  a{ 
    display: block;
    color: white;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none;
  }
  li a:hover {
    background-color: #111;
  }
  .active {
    background-color: #4CAF50;
  }
</style>
