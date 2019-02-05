<template>
  <div class="hello">
    <h1>Listado de usuarios</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p> 
    
  </div>
</template>

<script>
export default {
  name: 'Users',
  props: {
    msg: String
  } 
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped> 
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
