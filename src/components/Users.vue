<template>
  <div>
    <div class="userList" v-if="showForm==false">
      <div class="padding-container">
      <h1>User list</h1>
      <p>
        Simple system done with VueJS for rendering and making CRUD operations with Users got from JSONPlaceholder API.
      </p>
      <input v-model="textFilter" v-on:keyup="filter" class="input" placeholder="Search user"/>
      <br />
      </div>
      <a href="#!" class="btn-add-user" v-on:click="openForm(true, 'create')">+</a>
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
          <tr class="trBody" v-for="user in usersArray" v-bind:key="user.id" v-bind:data="user.id">
            <td><p class="letterImage">{{user.name[0]}}</p></td>
            <td>
              <h4 class="title text">{{user.name}}</h4>
              <p class="text text-data"><a class="at-email">@</a> {{user.email}}</p>
              <p class="text text-data"><img src="../assets/images/marker.png" alt="Marker" class="marker-img"> {{user.address && user.address.street}}, {{user.address && user.address.city}}</p>
              <p class="text text-data"><img src="../assets/images/phone.png" alt="Marker" class="marker-img"> {{user.phone}}</p>
            </td>
            <td class="text-center"><a class="text-link text-center" v-bind:href="'//' + user.website" target="blank">{{user.website}}</a></td>
            <td>
              <div class="well">
                <p class="text text-center text-companyName">{{user.company.name}}</p>
                <p class="text text-center text-slogan">"{{user.company.catchPhrase && user.company.catchPhrase}}"</p>
              </div>
            </td>
            <td>
              <div class="center">
                <a href="#!" class="btn" v-on:click="loadUser(user.id)">Edit</a>
                <a href="#!" class="btn" v-on:click="deleteUser(user.id)">Delete</a>
              </div>
            </td>
          </tr> 
        </tbody>
      </table>
      <div v-if="usersArray.length == 0">
        <p class="notFound">Users not found in this application</p>
      </div>
    </div>

    <UsersForm 
      v-bind:showForm="this.showForm"
      v-bind:userFormTitle="this.userFormTitle"
      v-bind:openForm="this.openForm"
      v-bind:currentUser="this.currentUser"
      v-bind:usersArray="this.usersArray"
      v-bind:overrideUsers="this.overrideUsers"/>
  </div>
</template> 


<script>
import "../assets/styles/usersComponent.css";
import axios from 'axios';
import UsersForm from './UsersForm.vue';

export default {
  name: 'Users',
  components: {
    UsersForm
  },
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
      currentUser: null, 
      textFilter: "",
      userFormTitle: ""
    }
  },
  methods:{
    deleteUser(id){ 
      this.$swal({
          title: 'Are you sure?',
          text: 'You can\'t revert your action',
          type: 'warning',
          showCancelButton: true,
          confirmButtonText: 'Yes Delete it!',
          cancelButtonText: 'No, Cancel!',
          showCloseButton: true,
          showLoaderOnConfirm: true
        }).then((result) => {
          if(result.value) {
            const usersList = JSON.parse(localStorage.getItem("USERS"));
            const userIndex = usersList.findIndex(r=>r.id == id);
            const newUserList = usersList.filter(r=>r.id !== usersList[userIndex].id);

            localStorage.setItem("USERS", JSON.stringify(newUserList))
            this.usersArray = JSON.parse(localStorage.getItem("USERS"));
            this.$swal('Deleted', 'You successfully deleted this file', 'success')
          }
        })

     
    }, 
    loadUser(id){
      this.openForm(true, "edit")
      this.currentUser = JSON.parse(localStorage.getItem("USERS")).filter(r => r.id == id)[0];
    },
    openForm(show, action){
      this.showForm=show;
      this.currentUser = null;
      if(action){
        if(action=="edit"){
          this.userFormTitle="Edit user"
        }else{
          this.userFormTitle="New user"
        }
      }
    },
    overrideUsers(array){
      this.usersArray=array;
    },
    filter(){
      const jsonArray = JSON.parse(localStorage.getItem("USERS"));
      const key = this.textFilter;
      this.usersArray = jsonArray.filter(function(item){
        return item.name.toLowerCase().indexOf(key.toLowerCase())>-1;
      });
    }
  }
}
</script> 