<template>
  
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-lg-10 col-sm-12 col-xs-12">
    <div class="center-content">
  <div class="centralPanel">
      <input type="text" placeholder="Inserta el nombre" id="name"> <br>
      <input type="button" class="btn-create-event" value="Busca tu película!!" @click="checkMovie()">
    </div>

    <!-- Setting v-if to iterate array 'posts' just after the user have called to external api rest from axios, it helps us to set reactive update of the film view and avoid errors -->
    
      <div class="ul-create-event container-fluid" v-if="posts&&posts.length">
        <div class="title-create-event ">{{posts[0].results[0].original_title}}</div>
         <div class="row d-flex justify-content-center film-create-event">
        <div class="col-lg-5 poster col-md-5 col-9">
        <img class="img-fluid" :src="'https://image.tmdb.org/t/p/w300' + posts[0].results[0].poster_path"></img>
        </div>
        <div class="col-lg-7 col-sm-12 col-md-12 col-12">
          <div class="row">
            <div class="col-12">
            <div class="overview">{{posts[0].results[0].overview}}</div>
            </div>
          </div>
        <div class="row">
          <div class="col-12">
          <div class="rate">Valoración: {{posts[0].results[0].vote_average}}</div>
          </div>
        </div>
        
        </div>
      </div>

  </div>

  <form class="form-inline justify-content-center">
    <label class="my-1 mr-2" for="inlineFormCustomSelectPref">Elige tu ciudad</label>
    <div class="wrapper">
    <select class="custom-select my-1 mr-sm-2" id="city">
        <option value='Alava'>Álava</option>
        <option value='Albacete'>Albacete</option>
        <option value='Alicante'>Alicante/Alacant</option>
        <option value='Almeria'>Almería</option>
        <option value='Asturias'>Asturias</option>
        <option value='Avila'>Ávila</option>
        <option value='Badajoz'>Badajoz</option>
        <option value='Barcelona'>Barcelona</option>
        <option value='Burgos'>Burgos</option>
        <option value='Caceres'>Cáceres</option>
        <option value='Cadiz'>Cádiz</option>
        <option value='Cantabria'>Cantabria</option>
        <option value='Castellon'>Castellón/Castelló</option>
        <option value='Ceuta'>Ceuta</option>
        <option value='Ciudadreal'>Ciudad Real</option>
        <option value='Cordoba'>Córdoba</option>
        <option value='Cuenca'>Cuenca</option>
        <option value='Girona'>Girona</option>
        <option value='Laspalmas'>Las Palmas</option>
        <option value='Granada'>Granada</option>
        <option value='Guadalajara'>Guadalajara</option>
        <option value='Gipuzcoa'>Guipúzcoa</option>
        <option value='Huelva'>Huelva</option>
        <option value='Huesca'>Huesca</option>
        <option value='Illes-bBalears'>Illes Balears</option>
        <option value='Jaen'>Jaén</option>
        <option value='A-coruña'>A Coruña</option>
        <option value='Larioja'>La Rioja</option>
        <option value='Leon'>León</option>
        <option value='Lleida'>Lleida</option>
        <option value='Lugo'>Lugo</option>
        <option value='Madrid'>Madrid</option>
        <option value='Malaga'>Málaga</option>
        <option value='Melilla'>Melilla</option>
        <option value='Murcia'>Murcia</option>
        <option value='Navarra'>Navarra</option>
        <option value='Ourense'>Ourense</option>
        <option value='Palencia'>Palencia</option>
        <option value='Pontevedra'>Pontevedra</option>
        <option value='Salamanca'>Salamanca</option>
        <option value='Segovia'>Segovia</option>
        <option value='Sevilla'>Sevilla</option>
        <option value='Soria'>Soria</option>
        <option value='Tarragona'>Tarragona</option>
        <option value='Santacruztenerife'>Santa Cruz de Tenerife</option>
        <option value='Teruel'>Teruel</option>
        <option value='Toledo'>Toledo</option>
        <option value='Valencia'>Valencia/Valéncia</option>
        <option value='Valladolid'>Valladolid</option>
        <option value='Vizcaya'>Vizcaya</option>
        <option value='Zamora'>Zamora</option>
        <option value='Zaragoza'>Zaragoza</option>
    </select>
    </div>
  </form>
      <!-- Datatime to offer user the oportunity to choose date and time, and we will close the
      cinema's bid comparating that time and actual time -->
    
  <input class="datetime-create-event" id="datetime" type="datetime-local"><br>
  <button class="addEvent" v-on:click='addEvent'>¡Añade el evento!</button>
</div>
</div>
</div>
</div>

</template>

<script>
import axios from 'axios';
import {dbEvents} from '../firebaseConfig.js';
import {db} from '../firebaseConfig.js';



export default {
  data() {
    return {
      posts: [],
      errors: []
    }
  },
  methods:{
    logout: function() {
      firebase.auth().signOut().then(() =>{
        this.$router.replace('Login')
      })
    },
    // starting axios request from film value input which users have choosen before.
    // key is my personal key offered by the API when I registered there. Must set to use the API

    checkMovie(){
      var film = document.getElementById("name").value;
      var key = `5ef8ac95eb987d939d52e931751fc791`
      axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${key}&query=${film}`)
      .then(response => {
           this.posts = []
           this.posts.push(response.data);
           console.log(posts.results);
           console.log(this.posts);   
      })
     .catch(e => {
      this.errors.push(e);
      })
    },
        // Changing the date format and saving it
        // myRef is getting the key of the new firebase object, then I save it to iterate the whole
        //object and work with it

      addEvent(){
      	   var date = document.getElementById("datetime").value;
      	  if (date == ''){
      	  	alert("Debes insertar una fecha y una hora para el evento")
      	  } else {


          var myRef = db.ref().push().getKey();
         
          var year = date.charAt(0)+date.charAt(1)+date.charAt(2)+date.charAt(3);
          var month = date.charAt(5)+date.charAt(6);
          var day = date.charAt(8)+date.charAt(9);
          var hour = date.charAt(11)+date.charAt(12);
          var minutes = date.charAt(14)+date.charAt(15);
          var hourEvent = hour + ":" + minutes;
          var dateEvent = day+'/'+month+'/'+year;


        let newEvent = {
          id: myRef,
          Title: this.posts[0].results[0].original_title,
          Poster: this.posts[0].results[0].poster_path,
          Overview: this.posts[0].results[0].overview,
          Rate: this.posts[0].results[0].vote_average,
          Date: dateEvent,
          Hour: hourEvent,
          Votes: 0,
          Bid:0,
          City: document.getElementById('city').value
        }
          dbEvents.child(myRef).set(newEvent);
          alert( (this.posts[0].results[0].original_title).toUpperCase() + ' ha sido añadido y ya puede ser votado!!')
          this.posts = [];
          document.getElementById("name").value = '';
          document.getElementById("datetime").value = '';
        }
    }
  },
}
</script>
</template>
<style scoped>

@media (min-width: 565px) {
    .overview { font-size: 3rem; }
   
}
@media (min-width: 56px) {
    .overview { font-size: 1rem; }
   
}




  .datetime-create-event{
    margin: 20px auto!important;
  }


  .title-create-event{
    font-size: 40px;
  }

  .ul-create-event{
    list-style-type: none;
  }

  .btn-create-event {
    margin-bottom: 20px;
  }



  #name{
    margin-top: 20px;
    margin-bottom: 20px;
  }


  .film-create-event {
    border: 1px solid #F3BD15;
    border-right: black;
    border-left: black;
    width: 100%;
    margin: 0;
  }

  .overview {
    width: 100%;
  }
  
  .wrapper {
    max-width: 256px;
  }

  select {
    width: 100%;
  }

  .poster {
    margin: 0.3rem 0;
  }

  .rate {
    font-size: 30px!important;
    padding-bottom: 20px;
    padding-top: 10px;
  }
  
  #datetime {
    width: 300px;
    height: 50px;
    font-weight: 50px;
  }

  .centraPanel {
    margin-top: 50px!important;
  }

  .center-content {
        width: 100%;
        background-color: #6a6a6a;
        border-radius: 5px;
        text-align: center;
        margin-bottom: 5rem;
        margin-top:0.4rem;
        }


  .addEvent {
    margin-bottom: 0.8rem;
  }

</style>