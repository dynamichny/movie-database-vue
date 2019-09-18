<template>
  <div id="app">
    <Bar @loginClicked="isLogin = $event" @registerClicked="isRegister = $event" :user="user" @logout="isLogout = $event"/>

    <Register v-if="isRegister" @register="registerData = $event" @closeRegister="isRegister = false"/>
    <Login v-if="isLogin" @login="loginData = $event" @closeLogin="isLogin = false"/>

    <transition name="modal">
      <Modal v-if="isModal" 
      :movie="showMovie" 
      :watchlist="watchlist" 
      @closeModal="isModal = $event" 
      @resetModalId="showMovieId = $event"
      @updateWatchlist="watchlist = $event" 
      />
    </transition>

    <transition name="wl">
      <Watchlist v-if="isWatchlist" 
      :watchlist="watchlist" 
      @closeWl="isWatchlist = $event" 
      @updateWatchlist="watchlist = $event"
      @modalId="showMovieId = $event"
      />
    </transition>

    <Nav :watchlist='isWatchlist' @changeWatchlist="isWatchlist = $event"/>
    <Header :randomMovie="randomMovie" @input="searchInput = $event" @showMovieId="showMovieId = $event" @modal="showMovieId = $event" />
    <TrendingWeek :trendingMovies="trendingMovies" @movieid="showMovieId = $event"/>
    <Footer />
    
  </div>
</template>

<script>
import Watchlist from './components/Watchlist.vue';
import Nav from './components/Nav.vue';
import Header from './components/Header.vue';
import TrendingWeek from './components/TrendingWeek.vue';
import Footer from './components/Footer.vue';
import Modal from './components/Modal.vue';
import Login from './components/Login.vue';
import Register from './components/Register.vue';
import Bar from './components/Bar.vue';
import db from './components/firebaseInit.js';
import firebase from 'firebase';

export default {
  name: 'app',
  components: {
    Nav,
    Header,
    TrendingWeek,
    Footer,
    Watchlist,
    Modal,
    Bar,
    Register,
    Login
  },
  data() {
    return {
      isWatchlist: false,
      randomMovie: {},
      trendingMovies: [],
      isModal: false,
      showMovie: {},
      showMovieId: '',
      watchlist: [],
      isLogin: false,
      isRegister: false,
      user: null,
      registerData: {},
      loginData: {},
      isLogout: false
    }
  },
  watch: {
    showMovieId: function() {
      if(this.showMovieId == '') return;
      fetch(`https://api.themoviedb.org/3/movie/${this.showMovieId}?api_key=485dd1f1ee71083619712efed20ee4bb&language=en-US`)
        .then(resp => resp.json())
        .then(resp => {
          this.showMovie = resp;
          this.isModal = true;
        });

    },
     watchlist: function() {
      if(this.user){
        let watchlist = this.watchlist;
        db.collection('watchlists').doc(this.user.uid).update({watchlist}).then(() => {
          console.log('updated')
        }).catch(error => alert(error))
      }
    }, 
    isLoginWGoogle: function(){
      firebase.auth().signInWithPopup().then(result =>{
        this.user = result.user;
      }).catch(error => alert(error))
    },
    registerData: function(){
      this.isRegister = false;
      firebase.auth().createUserWithEmailAndPassword(this.registerData.email, this.registerData.password).catch(error => alert(error))
    },
    loginData: function(){
      this.isLogin = false;
      firebase.auth().signInWithEmailAndPassword(this.loginData.email, this.loginData.password).then(result=>{
        this.user = result.user;
      }).catch(error => alert(error))
    },
    isLogout(){
      firebase.auth().signOut().then(() => {
        console.log('logged out')
        this.user = null;
        this.watchlist = [];
      }).catch(error => alert(error))
    },
    user: function(){
      if(this.user){
        db.collection('watchlists').doc(this.user.uid).get()
          .then(querySnapshot =>{
            if(querySnapshot.exists){
              querySnapshot.data().watchlist.forEach(movie => this.watchlist.push(movie))
            } else {
              db.collection('watchlists').doc(this.user.uid).set({
                watchlist: []
              });
            } 
          });
      }
    }
  },
  mounted() {
    fetch(`https://api.themoviedb.org/3/trending/movie/day?api_key=485dd1f1ee71083619712efed20ee4bb`)
      .then(resp => resp.json())
      .then(resp => {
        this.randomMovie = resp.results[Math.floor(Math.random() * resp.results.length)];
      });
    fetch(`https://api.themoviedb.org/3/trending/movie/week?api_key=485dd1f1ee71083619712efed20ee4bb`)
      .then(resp => resp.json())
      .then(resp => {
        this.trendingMovies = resp.results;
      });
  },
}
</script>

<style lang="scss">
html{
  font-size: 62.5%
}
body{
  margin: 0;
  padding: 0;
  background: #181818;
  font-family: 'Source Code Pro', monospace;
}
.wl-enter, .wl-leave-to {
  transform: translateX(100%);
}
.wl-enter-active, .wl-leave-active{
  transition: transform 0.5s cubic-bezier(.69,.5,0,1);
}

.modal-enter, .modal-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}
.modal-enter-active, .modal-leave-active{
  transition: opacity 0.5s linear, transform 0.5s ease-in-out;
}
</style>
