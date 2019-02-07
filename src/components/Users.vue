<template>
  <div>




    <div class="userList" v-if="showForm==false">
      <h1>User list</h1>
      <p>
        Simple system done with VueJS for rendering and making CRUD operations with Users got from JSONPlaceholder API.
      </p>  
      <a href="#!" class="btn-add-user" v-on:click="getForm()">+</a>
      <table class="table" v-if="usersArray.length > 0">
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
          <tr class="trBody" v-for="user in usersArray" v-bind:key="user.id">
            <td><p class="letterImage">{{user.name[0]}}</p></td>
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
                <a href="#!" class="btn" v-on:click="loadUser(user.id)">Editar</a>
                <a href="#!" class="btn" v-on:click="deleteUser(user.id)">Eliminar</a>
              </div>
            </td>
          </tr> 
        </tbody>
      </table>
      <div v-if="usersArray.length == 0">
        <p class="notFound">Users not found in this application</p>
      </div>
    </div>




    <div class="userForm" v-if="showForm==true">
      <h1>New User</h1>
      <p>
        You can create a new User with his personal information right here!.
      </p>

        <p v-if="errors.length">
          <b>Please correct the following error(s):</b>
          <ul>
            <li v-for="error in errors" v-bind:key="error.id">{{ error }}</li>
          </ul>
        </p>
      <form 
        @submit="manageUser" 
        class="userFormFor"
        method="post"> 

        <label class="label" for="userName">User name</label>
        <br />
        <input
          id="userName"
          v-model="userName"
          type="text"
          name="userName"
          class="input"
        > 
        <br />
        
        <label class="label" for="userEmail">Email </label>
        <br />
        <input
          id="userEmail"
          v-model="userEmail"
          type="text"
          name="userEmail"
          class="input"
        >
        <br />
        
        <label class="label" for="userWebsite">Website</label>
        <br />
        <input
          id="userWebsite"
          v-model="userWebsite"
          type="text"
          name="userWebsite"
          class="input"
        >
        <br />
        
        <label class="label" for="company.companyName">Company Name</label>
        <br />
        <input
          id="company.companyName"
          v-model="company.companyName"
          type="text"
          name="company.companyName"
          class="input"
        >
        <br />
        
        <label class="label" for="company.companyCatchPhrase">Company Catch Phrase</label>
        <br />
        <textarea
          id="company.companyCatchPhrase"
          v-model="company.companyCatchPhrase"
          type="text"
          name="company.companyCatchPhrase"
          class="input">
        </textarea> 
        <br />
        
        <input
          type="submit"
          value="Create user"
          class="btn-submit-user"
        > 
        <a href="#!" v-on:click="showForm=false" class="btn-cancel-user">Cancel</a>
      </form>
    </div>
  </div>
  
</template> 


<script>
import "../assets/styles/usersComponent.css";
import axios from 'axios';

export default {
  name: 'Users',
  mounted(){
    axios
      .get('https://jsonplaceholder.typicode.com/users')
      .then(response => { 
        const usersList = localStorage.getItem("USERS");
        if(!usersList){
          localStorage.setItem("USERS", JSON.stringify(response.data))
        }
        this.usersArray = JSON.parse(localStorage.getItem("USERS"));
      })
      .catch(error => console.log(error))
  },
  data(){
    return{
      usersArray: [],
      showForm: false,
      errors: [],
      userId: null,
      userName: null,
      userEmail: null,
      userWebsite: null,
      company: {
        companyName: null,
        companyCatchPhrase: null,
      }
    }
  },
  methods:{
    initVariables(){
      return { 
        userId: null,
        userName: null,
        userEmail: null,
        userWebsite: null,
        company: {
          companyName: null,
          companyCatchPhrase: null,
        }
      }
    },
    deleteUser(id){ 
      const usersList = JSON.parse(localStorage.getItem("USERS"));
      const userIndex = usersList.findIndex(r=>r.id == id);
      const newUserList = usersList.filter(r=>r.id !== usersList[userIndex].id);

      localStorage.setItem("USERS", JSON.stringify(newUserList))
      this.usersArray = JSON.parse(localStorage.getItem("USERS"));
    },
    manageUser: function (e) {
      e.preventDefault();
      if(this.userId == null){
        if (this.userName && this.userEmail) { 
          const usersList = JSON.parse(localStorage.getItem("USERS"));
          usersList.unshift({
            id: this.usersArray.length + 1,
            name: this.userName,
            email: this.userEmail,
            website: this.userWebsite,
            company: {
              name: this.company.companyName,
              catchPhrase: this.company.companyCatchPhrase
            }
          });

          localStorage.setItem("USERS", JSON.stringify(usersList))
          this.usersArray = JSON.parse(localStorage.getItem("USERS"));
          this.showForm=false;
        }else{
          if (!this.userName) {
            this.errors.push('Name required.');
          }
        }
      }else{
        alert("editando....")
      }
      
    },
    getForm(){
      this.showForm=true;
      this.initVariables()
    },
    loadUser(id){
      this.showForm = true,
      this.userId = id;
      const currentUser = JSON.parse(localStorage.getItem("USERS")).filter(r => r.id == id);  
      this.userName=currentUser[0].name;
      this.userEmail=currentUser[0].email;
      this.userWebsite=currentUser[0].website;
      this.company.companyName=currentUser[0].company.name;
      this.company.companyCatchPhrase=currentUser[0].company.catchPhrase;
    }
  }
}
</script> 