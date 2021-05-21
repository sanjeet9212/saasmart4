<template>
  <div class="home-page">
    <h1>Enter your Details</h1>
    <div class="home-page-form">
      <form @submit="postData" method="post" class="form-group">
        <input type="text" name="name" class="input"
          placeholder="Enter your Name" 
          autocomplete="OFF"
        v-model="posts.name">

        <input type="email" name="name" class="input"
          placeholder="Enter your Email" 
          autocomplete="OFF"
        v-model="posts.email">
        <button type="submit" class="submit-button">Submit</button>
      </form>
    </div>

    <div class="get-details">
      <div v-for="person in user" :key="person.id" class="show-data">
          <h4>Name: {{person.name}}</h4>
          <h4>Email: {{person.email}}</h4>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios)
import "./InputForm.css";
export default {
  name: "InputForm",
  data(){
    return{
        posts:{
          name:"",
          email:""
        },
        user:[]
    }
  },
  created() {
    fetch("http://localhost:8080/api/v1/users")
    .then(res => {
      return res.json();
    })
    .then(data => {
      this.user = data;
    });
  },
  methods:{
    postData(e)
    {
      if (this.posts.name!=='' && this.posts.email!==''){
        this.axios.post("http://localhost:8080/api/v1/users", this.posts)
        .then((res) => {
          console.warn(res)
          this.posts.name=''
          this.posts.email=''
        })
      }else{
        alert("Enter valid Input")
      }
      e.preventDefault();
      window.location.reload();
    }
  }
};
</script>