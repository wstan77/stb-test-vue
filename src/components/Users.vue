<template>
  <div class="hello">
    <h1>User list</h1>
    <p>
      Simple system done with VueJS for rendering and making CRUD operations with Users got from JSONPlaceholder API.
    </p> 

    
    <a href="" class="btn-add-user">+</a>
    <table class="table">
      <thead class="thead-header">
        <tr>
          <th></th>
          <th>User</th>
          <th>Website</th>
          <th>Company</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr class="trBody">
          <td><p class="letterImage">L</p></td>
          <td>
            <h4 class="title text">Leanne Graham</h4>
            <p class="text text-data"><a class="at-email">@</a> Sincere@april.biz</p>
            <p class="text text-data"><img src="../assets/images/marker.png" alt="Marker" class="marker-img"> Kulas Light, Gwenborough</p>
          </td>
          <td><a class="text-link" href="asdad" target="blank">hildegard.org</a></td>
          <td>
            <div class="well">
              <p class="text text-center text-companyName">Romaguera-Crona</p>
              <p class="text text-center text-slogan">"Multi-layered client-server neural-net"</p>
            </div>
          </td>
          <td>
            <a href="" class="btn">Editar</a>
            <a href="" class="btn">Eliminar</a>
          </td>
        </tr> 
      </tbody>
    </table>
  </div>
</template>

<script>
import "../assets/styles/usersComponent.css"
export default {
  name: 'Users',
  props: {
    msg: String
  } 
}
</script> 