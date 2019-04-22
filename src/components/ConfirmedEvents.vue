<template>
  <div>
    <div class="container">
      <div class="row justify-content-center">
    <div class="col-lg-3 mt-1 mb-1 col-md-3 col-9 d-flex justify-content-center film text-center" v-for="film in Films"> <!-- This v-for allow us to iterate each film which we saved in Films array -->
      <div class="films">
      <div id="film">
        <div class="title-confirmed justify-content-center d-flex align-items-center font-weight-bold">{{film.Title}}</div>
        <div class="cityDate d-flex align-items-center font-weight-bold justify-content-center">{{film.City}} - {{film.Date}} - {{film.Hour}}</div>
        <img class="img-fluid" :src="'https://image.tmdb.org/t/p/w300' + film.Poster"></img>
       <!-- <div id="overview">{{film.Overview}}</div>-->
        <div id="rate" v-model= "film.Rate"></div>
      </div>
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
import {dbConfirmedEvents} from '../firebaseConfig.js';
import {db} from '../firebaseConfig.js';
import firebase from 'firebase';

export default {
  data () {
      return {
       Films: []
      }
    },
    created (){ // pushing to the array each film which has finished cinemas biding. 
        dbConfirmedEvents.on('child_added', snapshot=> this.Films.push(snapshot.val()))
    }
    
  }

</script>

<style scoped>
  
  body{
    background-color: black;
  }

  .title-confirmed{
    font-size: 1.5rem;
    color: #323232;
    text-shadow: 0 0 0.16rem #323232;
    min-height: 89.33px;
  }

  

  .cityDate{

    min-height: 60px;
    align-items: center;
    align-content: center;
    margin: 0 0.2rem;
    color: #323232;
  }

  #films{
    width: 100%;
    margin: 0 auto;
  }

  #film{
    border: 1px solid #323232;
    border-radius: 5px;
    background-color: black;
    color: #F3BD15;
  }

  


</style>