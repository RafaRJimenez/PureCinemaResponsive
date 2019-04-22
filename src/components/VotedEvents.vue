<template>
  <div>
    <div class="container">
      <div class="row justify-content-center">
    <div class="col-lg-3 col-md-3 col-9 d-flex justify-content-center film text-center" v-for="film in Films">
      <div class="single">
        <div class="title d-flex align-items-center justify-content-center" id="title">{{film.Title}}</div>
         <div class="dateCity font-weight-bold" v-model="film.Date">{{film.City}} - {{film.Date}} - {{film.Hour}}</div>
        <img class="img-fluid" :src="'https://image.tmdb.org/t/p/w300' + film.Poster"></img>
       <!--<div class="overview" id="overview">{{film.Overview}}</div>-->
       <div class="rate-voted">{{film.Rate}}</div>
        <input class="bid" v-model="film.Bid"> Puja actual </input>
        <nav class="biding justify-content-center">
          <ul>
            <li v-on:click='biding(film, 5)'> +5 €</li>
            <li v-on:click='biding(film, 10)'> +10 €</li>
            <li v-on:click='biding(film, 20)'> +20 €</li> 
          </ul>
      </nav>
     
      </div>
    </div>
  </div>
  </div>
  </div>
 
</template>

<script>
import {dbEvents} from '../firebaseConfig.js';
import {dbVotedEvents} from '../firebaseConfig.js';
import {db} from '../firebaseConfig.js';
import firebase from 'firebase';
import {dbUsers} from '../firebaseConfig.js';
import {router} from "../main.js";
import {dbConfirmedEvents} from '../firebaseConfig.js';
//dbEvents.on('child_added', snapshot=> this.Film.push(snapshot.val()))

export default {

	/*

	Here were are in biding place of application, so just cinema's user can use this component, 
	to prevent it we are checking the user role to either let him come inside or link him to 'home'

	 */

 beforeCreate(){
  var user = firebase.auth().currentUser.uid;
  dbUsers.orderByChild("id").equalTo(user).on("value", function(snapshot) {
   	 snapshot.forEach(function(childSnapshot) {
        var key = childSnapshot.key;
        var childData = childSnapshot.val();
        var role = childData.role;
        if (role!="Cine"){
             alert("NO ESTÁS AUTORIZADO PARA VER ESTE CONTENIDO, SERÁS REDIGIRIDO")
             router.go(0);
        } else {
        }
   	 });
   });
 },
  data () {
      return {
       Films: [],
      }
    },
    created (){
    	
    	/* after call the firebase database, we are pushing to the array 'Films' the films 
    	which got 100 votes from users, so they are ready to start their bid.

    	*/

        dbVotedEvents.on('child_added', snapshot=> this.Films.push(snapshot.val()));
    },
    updated(){

    	/* Now we are checking the date of each film, so if it is the same as the actual date they are removed from the array 'Films' and therefore they are removed from user frontend view */

       this.Films.forEach(function(film, index, object) {
            var date = day();
            if (date == film.Date){
                object.splice(index, 1);
            }
        });
      today();

      // after removing from our actual array, we iterate each film from the database to check the date and add the film to the new database 'confirmedEvents' if this is needed, at the same time we remove the film from its actual dabatase 'votedEvents'

      function today(){
          var todayDate = day();
           dbVotedEvents.orderByChild("Date").equalTo(todayDate).on("value", function(snapshot) {
           snapshot.forEach(function(childSnapshot) {
            var key = childSnapshot.key;
            var currentRef = db.ref('VotedEvents/' + key);
            currentRef.remove();
            var childData = childSnapshot.val();
            dbConfirmedEvents.push(childData);
            var name = childData.Title;
           });
        });
      };
      
      function day(){
         var today = new Date();
          var dd = today.getDate();
          var mm = today.getMonth()+1; 
          var yyyy = today.getFullYear();
          if(dd<10){
            dd='0'+dd;
          } 
          if(mm<10){
          mm='0'+mm;
         } 
         var todayDate = dd+'/'+mm+'/'+yyyy;
         return todayDate;
      }
    },

    /* Through the method 'biding' we allow the users cinema to raise the bid, it is 
    updated to the frontend through the array and to the single event database too, so each film is saving only the higest bid */

    methods: {
          biding: function(film, bid){
            film.Bid = film.Bid + bid;
            dbVotedEvents.child(film.id).child("Bid").set(film.Bid);
    	  } ,
    }
}



</script>

<style scoped>

    .biding{
     

      }

    .biding ul li {
       border: 1px solid black;
       cursor: pointer;
       padding: 5px;
       margin-bottom: 0.5rem;
        display: inline-block;
    }

    ul {
      padding: 0.7rem 0;
    }

    .bid {
      width: 50px;
      text-align: center;

    }

    .title {
      min-height: 60px;
    }

    .img-fluid {
      max-width: 6rem;
    }


   

  .film{
    border: 1px solid #F3BD15;
    color: black;
    background-color: #6A6A6A;
  }

  .row {
    margin: 0 auto;
  }

  .rate-voted{
   
    color: #6A6A6A;
  }

  
</style>