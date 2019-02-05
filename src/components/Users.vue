<template>
  <div class="hello">
    <h1>User list</h1>
    <p>
      Simple system done with VueJS for rendering and making CRUD operations with Users got from JSONPlaceholder API.
    </p>  
    <a href="#!" class="btn-add-user">+</a>
    <table class="table" v-if="info.length > 0">
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
          <tr class="trBody" v-for="user in info" v-bind:key="user.id">
            <td><p class="letterImage">L</p></td>
            <td>
              <h4 class="title text">{{user.name}}</h4>
              <p class="text text-data"><a class="at-email">@</a> {{user.email}}</p>
              <p class="text text-data"><img src="../assets/images/marker.png" alt="Marker" class="marker-img"> Kulas Light, Gwenborough</p>
            </td>
            <td><a class="text-link" v-bind:href="'//' + user.website" target="blank">{{user.website}}</a></td>
            <td>
              <div class="well">
                <p class="text text-center text-companyName">{{user.company.name}}</p>
                <p class="text text-center text-slogan">"{{user.company.catchPhrase && user.company.catchPhrase}}"</p>
              </div>
            </td>
            <td>
              <div class="center">
                <a href="" class="btn">Editar</a>
                <a href="#!" class="btn" v-on:click="deleteUser(user.id)">Eliminar</a>
              </div>
            </td>
          </tr> 
      </tbody>
    </table>
    <div v-if="info.length == 0">
      <p class="notFound">Users not found in this application</p>
    </div>
  </div>
</template>

<script>
import "../assets/styles/usersComponent.css";
import axios from 'axios';

export default {
  name: 'Users',
  props: {
    connection: String
  },
  mounted(){
    axios
      .get('https://jsonplaceholder.typicode.com/users')
      .then(response => { 
        const usersList = localStorage.getItem("USERS");
        if(!usersList){
          localStorage.setItem("USERS", JSON.stringify(response.data))
        }
        this.info = JSON.parse(localStorage.getItem("USERS"));
      })
      .catch(error => console.log(error))
  },
  data(){
    return{
      info: []
    }
  },
  methods:{
    deleteUser(id){ 
      const usersList = JSON.parse(localStorage.getItem("USERS"));
      const userIndex = usersList.findIndex(r=>r.id == id);
      const newUserList = usersList.filter(r=>r.id !== usersList[userIndex].id);
      localStorage.setItem("USERS", JSON.stringify(newUserList))
      this.info = JSON.parse(localStorage.getItem("USERS"));
    }
  }
}
</script> 