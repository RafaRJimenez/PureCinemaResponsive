<template>
  <header class="container">
    <div class = "navbar navbar-toggleable-md navbar-light justify-content-md-center row">
        <div class=" col-md-2 inicio text-center"><a href="/" class="goHome">Inicio</a></div>
        <div class="col-md-4 text-center">
          <router-link :to = "{name:'registerUserLink'}"> <div class="user-register p-2 m-2 h6"> ¡Regístrate para crear eventos!</div></router-link>
        </div>
        <div class="col-md-4 text-center">
          <router-link :to = "{name:'registerUserCineLink'}"> <div class="cine-register p-2 m-2 h6"> Registro para gerentes de cine</div></router-link>
        </div>
        <div class=" col-md-2 text-center">
          <router-link :to = "{name:'loginUserLink'}" class="login-li"><a href="#" class="btn login-header"> Login</a> </router-link>
        </div>
    </div>
  </header>

</template>

<style scoped>
  body {
    background-color: white;
    color: white;
    font-family: 'Russo One';
  }

  .col-md-4, .col-md-2{
  }

  header{
  margin-bottom: 20px;
  font-size: 1.5em;
  width: 100%;
  background-color: black;
  }

  .user-register{
    background-color: black;
    color: #F3BD15;
    text-decoration: none;
    border: 1px solid #F3BD15;
    border-radius: 10px;
  }

  .login-header{
    background-color: black;
    border: 1px solid #F3BD15;
    color: #F3BD15;
  }

  .login-header:hover{
    color: #F3BD15;
  }

  a{
    text-decoration: white;
  }

   a:hover{
    text-decoration: white;
  }


  .login-li{
  }

  .user-register:hover{
   background-color: #FFDF00;
   color: black;
   text-decoration: 
  } 

    .cine-register{
    background-color: black;
    color: #F3BD15;
    text-decoration: none;
    border: 1px solid #F3BD15;
    border-radius: 10px;
  }

  .cine-register:hover{
    background-color: #FFDF00;
    color: black;
    text-decoration: none;
  } 

  .inicio {
    margin-bottom: 0.2rem;
  }


  .goHome{
    color: black;
    background-color: #F3BD15;
    width: 96px;
    height: 47px;
    text-align: center;
    vertical-align: middle;  
    border-radius: 5px;
    font-size: 1.2rem;
    padding: 0.4rem;

  }


</style>