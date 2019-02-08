<template>
   <div class="userForm" v-if="showForm==true">
     <div class="padding-container">
       <h1>{{userFormTitle}}</h1>
        <img alt="AddUser" src="../assets/images/add-user.png" class="add-user-image">
        <p>
          You can create and edit Users with his personal information right here!.
        </p>
     </div> 
      <div v-if="errors.length">
        <p><b>Please correct the following error(s):</b></p> 
        <p v-for="error in errors" v-bind:key="error.id" class="validation-item">{{ error }}</p> 
      </div>
      <form 
        @submit="manageUser" 
        class="userFormFor"
        method="post"> 

        <label class="label" for="userName">User name</label>
        <br />
        <input
          id="userName"
          v-model="user.name"
          type="text"
          name="user.name"
          class="input"
        > 
        <br />
        
        <label class="label" for="email">Email </label>
        <br />
        <input
          id="email"
          v-model="user.email"
          type="text"
          name="user.email"
          class="input"
        >
        <br />
        
        <label class="label" for="website">Website</label>
        <br />
        <input
          id="website"
          v-model="user.website"
          type="text"
          name="user.website"
          class="input"
        >
        <br />

        <label class="label" for="address.street">Street</label>
        <br />
        <textarea
          id="address.street"
          v-model="user.address.street"
          type="text"
          name="user.address.street"
          class="input">
        </textarea>


        <br />

        <label class="label" for="address.city">City</label>
        <br />
        <select
          id="address.city"
          v-model="user.address.city" 
          name="user.address.city"
          class="input"> 
          <option v-for="city in cities" v-bind:key="city" v-bind:value="city">{{city}}</option>
        </select>
        <br />

        <label class="label" for="phone">Phone Number</label>
        <br />
        <input
          id="phone"
          v-model="user.phone"
          type="text"
          name="user.phone"
          class="input"
        >
        <br />
        
        <label class="label" for="company.name">Company Name</label>
        <br />
        <input
          id="company.name"
          v-model="user.company.name"
          type="text"
          name="user.company.name"
          class="input"
        >
        <br />
        
        <label class="label" for="company.catchPhrase">Company Catch Phrase</label>
        <br />
        <textarea
          id="company.catchPhrase"
          v-model="user.company.catchPhrase"
          type="text"
          name="user.company.catchPhrase"
          class="input">
        </textarea> 
        <br />
        
        <input
          type="submit"
          value="Save"
          class="btn-submit-user"
        > 
        <a href="#!" v-on:click="cleanForm()" class="btn-cancel-user">Cancel</a>
      </form>
    </div>
</template>

<script>
export default {
  name: "UsersForm",
  props: {
    showForm: Boolean,
    openForm: Function,
    currentUser: Object,
    usersArray: Array,
    overrideUsers: Function,
    userFormTitle: String
  },
  beforeUpdate(){
    this.user.id = this.currentUser==null ? null : this.currentUser.id;
    this.user.name = this.currentUser==null ? (this.user.name || null) : (this.user.name || this.currentUser.name)
    this.user.email = this.currentUser==null ? (this.user.email || null) : (this.user.email || this.currentUser.email)
    this.user.website = this.currentUser==null ? (this.user.website || null) : (this.user.website || this.currentUser.website)
    this.user.phone = this.currentUser==null ? (this.user.phone || null) : (this.user.phone || this.currentUser.phone)
    this.user.company.name = this.currentUser==null ? (this.user.company.name || null) : (this.user.company.name || this.currentUser.company.name)
    this.user.company.catchPhrase = this.currentUser==null ? (this.user.company.catchPhrase || null) : (this.user.company.catchPhrase || this.currentUser.company.catchPhrase)
    this.user.address.street = this.currentUser==null ? (this.user.address.street || null) : (this.user.address.street || this.currentUser.address.street)
    this.user.address.city = this.currentUser==null ? (this.user.address.city || null) : (this.user.address.city || this.currentUser.address.city)
  },
  data(){
    return {
      errors: [],
      cities: [
        "Aliyaview", 
        "Bartholomebury",
        "California",
        "Gwenborough", 
        "Howemouth",
        "Ilopango",
        "Lebsackbury",
        "McKenziehaven", 
        "Melbourne",
        "Oslo",
        "Roscoeview", 
        "San Salvador",
        "Santa Tecla",
        "South Christy",
        "South Elvis", 
      ],
      user:{
        id: null,
        name: null,
        email: null,
        website: null,
        phone: null,
        company: {
          name: null,
          catchPhrase: null
        },
        address: {
          street: null,
          city: null
        }
      }
    }
  },
  methods: {

    manageUser: function (e) {      
      e.preventDefault();
      const usersList = JSON.parse(localStorage.getItem("USERS"));
      if (this.user.name && this.user.email) { 
        if(this.user.id == null){
          usersList.unshift({
            id: this.usersArray.length + 1,
            name: this.user.name,
            email: this.user.email,
            website: this.user.website,
            phone: this.user.phone,
            company: {
              name: this.user.company.name,
              catchPhrase: this.user.company.catchPhrase
            },
            address: {
              street: this.user.address.street,
              city: this.user.address.city
            }
          });
          this.changeArray(usersList);
        }else{ 
          const userIndex = usersList.findIndex(r => r.id ==this.user.id); 
          usersList[userIndex] = this.user; 
          this.changeArray(usersList);
          
        }
        this.cleanForm();
      }else{
        if (!this.user.name) {
          this.errors.push('Name required.');
        }
        if (!this.user.email) {
          this.errors.push('Email required.');
        }
      }
    },
    cleanForm(){  
      
      Object.keys(this.user ? this.user : {}).reduce((r, k) => { 
        if(this.user[k] != null){
          if(typeof this.user[k]=="object"){
            Object.keys(this.user[k]).reduce((r, k1) => { 
              this.user.company.name = null;  
              this.user.company.catchPhrase = null;  
              this.user.address.street = null;  
              this.user.address.city = null;  
            })
          } else{
            if(k!="id"){
              this.user[k] = null; 
            }
          }
        }
      });
      this.openForm(false);
    },
    changeArray(array){
      localStorage.setItem("USERS", JSON.stringify(array))
      this.overrideUsers(JSON.parse(localStorage.getItem("USERS"))); 
    }
  }
}
</script>
