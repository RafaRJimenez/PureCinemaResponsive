<template>
  <div>
    <div class="row justify-content-center text-center">
    <div class="col-lg-3 col-md-3 col-9 d-flex justify-content-center" v-for="film in Films">
      <div class="films">
      <div class="show-film">
        <div class="title text-capitalize font-weight-bold">{{film.Title.toUpperCase()}}</div>
        <div class="cityDate font-weight-bold">{{film.City}} - {{film.Date}} - {{film.Hour}}</div>
        <img class="img-fluid" id="poster" :src="'https://image.tmdb.org/t/p/w300' + film.Poster"></img>
       <!-- <div id="overview">{{film.Overview}}</div>-->
        <div class="rate-vote-event" v-model= "film.Rate"></div>
        <div class="font-weight-bold" id="votes">Votos: <span> {{film.Votes}} </span></div>
        <div class="addVotes" v-on:click='voteEvent(film)'>Quiero ir a este evento</div>
      </div>
    </div>
    </div>
</div>
  </div>
 
</template>

<script>
import {dbUsers} from '../firebaseConfig.js';
import {dbEvents} from '../firebaseConfig.js';
import {dbVotedEvents} from '../firebaseConfig.js';
import {db} from '../firebaseConfig.js';
import firebase from 'firebase';
//dbEvents.on('child_added', snapshot=> this.Film.push(snapshot.val()))

export default {
  data () {
      return {
       Films: []
      }
    },

    	// Pushing to the array all the events which have been built by the users, to start their votation

    created (){
        dbEvents.on('child_added', snapshot=> this.Films.push(snapshot.val()))
    },


    	/* The method 'voteEvent' is in charge of the votation system of the events. It includes add a new vote from each user if they are interested in it, so the 'forEach' bucle check all users that are included to the event, if they are inside the vote is not included, if they don`t the vote is added to the fronted from the array and updated in the database. 
		*/
		

    methods: {
          voteEvent: function(film){
            dbEvents.child(film.id).child("Votes").set(parseInt(film.Votes)+1);
            film.Votes ++;
            var check = "none";
            var currentRef = db.ref('events/' + film.id);
            var user = firebase.auth().currentUser.uid;
            db.ref("events/"+ film.id +"/users").once("value", function(snapshot) {
            	snapshot.forEach(function(childSnapshot) {
               		var key = childSnapshot.key;
            		var childData = childSnapshot.val();
            		if (childData == user){
              		check = "created";
               		dbEvents.child(film.id).child("Votes").set(parseInt(film.Votes)-1);
               		film.Votes--;
            		} 
           		 });
          	});

            	/*
    			Finally we check if the event has received 100 votes, in that case we update everything again and move the event from 'voteEvent' database to 'votedEvents'.
    			*/

              if (check == "none"){
                var currentRef = db.ref('events/' + film.id +'/users');
             	 currentRef.push(user);
           	 		if (film.Votes == 100){
               			 var currentRef = db.ref('events/' + film.id);
                		 currentRef.remove();
               		     this.Films.splice(this.Films.indexOf(film), 1);
                         var myRef = db.ref().push().getKey();
                         film.id= myRef;
                         dbVotedEvents.child(myRef).set(film);
                    }
                    else {
                       alert("Ya estás apuntado a este evento!!");
                    }
   			  }	
          }
    }
    
}

</script>

<style scoped>
  
  body{
    background-color: black;
  }

  .row {
    margin-top: 0.7rem;
  }

  #addVotes{
    margin: 50px 200px 50px 0;
    float: none;
    height: 400px;
    font-size: 15px;
    border-radius: 5px;
    cursor: pointer;
  }

  #votes{
    width: 70px;
    text-align: center;
    height: 40px;
    margin: 20px auto;
  }

  .cityDate{
    color: #323232;
  }
 /*
  #overview{
    font-size: 15px!important;
    margin: 20px 45px 20px;
    padding: 50px 0;
    border-bottom: 1px solid yellow;
  }
  */
  #films{
    width: 100%;
    margin: 0 auto;
  }
  .show-film{
  
  }

  #overview{
    width: 400px;
    float: right;
  }

  .rate-vote-event{
    float: right;
    width: 140px;
    height: 30px;
    position: absolute;
    font-weight: 50px;
    margin: -31px 525px;
  }

  .img-fluid {
    padding: 1rem;
  }


  .title {
    text-transform: capitalize;
    color: #323232;
    text-shadow: 0 0 0.16rem #323232;
    font-size: 1.5rem;
  }

  span {
    color: #F3BD15;
  }

  
  #datetime{
    width: 300px;
    height: 50px;
    font-weight: 50px;
  }

  .addVotes{
    color: #F3BD15;
    width: 100px;
    height: 50px;
    background-color: black;
    margin: 40px auto;
    font-size: 15px;
    padding-top: 2px;
    border-radius: 5px;
    cursor: pointer;
    border-left: 1px solid #F3BD15;
    border-right: 4px solid #323232;
  }

  .d-flex {
    
    border: 0.01rem solid #323232;
  }

</style>