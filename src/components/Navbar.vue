<template>
<!--
	<div>
    <div class="container">
    <nav class="row vertical-align justify-content-md-center">
      <div class="col-md-2 p-2 col-sm-12 align-middle text-center">
        <router-link :to = "{name:'createEventLink'}">Crea un evento</router-link>
      </div>
      <div class="col-md-2 p-2 col-sm-12 text-center">
         <router-link :to = "{name:'voteEventLink'}">Únete a un evento!</router-link>
      </div>
      <div class="col-md-2 p-2 col-sm-12 text-center">
         <router-link :to = "{name:'VotedEventsLink'}">Eventos en subasta</router-link>
      </div>
      <div class="col-md-3 p-2 col-sm-12 text-center">
         <router-link :to = "{name:'confirmedEventsLink'}">Eventos confirmados</router-link>
      </div>
      <div class="col-md-2 p-2 col-sm-12 text-center">
        <a v-on:click='logout' class=""  href="#">CERRAR SESIÓN</a>
      </div>
</nav>   
</div>

        	</div>
        -->
        
        <div>

        <nav class="navbar navbar-expand-sm d-flex justify-content-center">
  <button class="navbar-toggler navbar-dark" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon navbar-dark"></span>
  </button>

  <div class="collapse navbar-collapse text-center" id="navbarSupportedContent">
    <ul class="navbar-nav justify-content-center align-middle mr-auto d-flex align-items-center">
      <li class="nav-item active">
       <a class="nav-link" href="#">  <router-link class="text-center justify-content-center" :to = "{name:'createEventLink'}">Crea un evento</router-link> </a>
      </li>
       <li class="nav-item  active">
        <a class="nav-link" href="#"> <router-link :to = "{name:'voteEventLink'}">Únete a un evento!</router-link></a>
      </li>
       <li class="nav-item active">
        <a class="nav-link" href="#"> <router-link :to = "{name:'VotedEventsLink'}">Eventos en subasta</router-link></a>
      </li>
       <li class="nav-item active">
        <a class="nav-link" href="#">  <router-link :to = "{name:'confirmedEventsLink'}">Eventos confirmados</router-link></a>
      </li>
       <li class="nav-item active ">
        <a v-on:click='logout' class="nav-link"  href="#">CERRAR SESIÓN</a>
      </li>
     
    </ul>
  </div>
</nav>
</div>
</template>

<script>



import axios from 'axios';
import firebase from 'firebase';
var database;

export default {
  data() {
    return {
    }
  },
  methods:{
    logout: function() {
      firebase.auth().signOut().then(() =>{
        this.$router.replace('Login');
      })
    }
  }
}
</script>

<style scoped>

body{
      background-color: black;
      font-family: 'Russo One';
  }
  

.container{
  background-color: #323232;
  border-radius: 15px;
  margin-top: 2rem;
  margin-bottom: 2rem;
}

  body{
      background-color: black;
  }
  
  ul li {

    margin: 0.3rem auto;

}

nav {
  margin: 0 1rem;
  background-color: #323232;
  border-radius: 0.7rem;
  
}



a {
  margin: 0 auto;
  color: #F3BD15;
  
}

.navbar-nav {
    float:none;
    margin:0 auto;
    /*
    display: block;
    text-align: center;
    */
}

  
  .navbar-edit{
    background-color: #323232!important;
    color: #F3BD15!important;
    margin: 50px!important;
    border-radius: 5px;
    text-align: center;
    vertical-align: middle; 
  }

  .nav-link{
    color: #F3BD15!important;
   /* border-right: 1px solid #F3BD15;
    border-left: 2px solid #F3BD15;*/
  }


  .navbar-toggler {
    border: 1px solid #F3BD15;

  }


  .vertical-align {
    display: flex;
    align-items: center;
}


</style>