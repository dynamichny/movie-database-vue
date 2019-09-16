<template>
  <div id="app">
    <transition name="modal">
      <Modal v-if="isModal" 
      :movie="showMovie" 
      @closeModal="isModal = $event" 
      @resetModalId="showMovieId = $event"
      :watchlist="watchlist" 
      @updateWatchlist="watchlist = $event" 
      />
    </transition>

    <transition name="wl">
      <Watchlist v-if="isWatchlist" 
      @closeWl="isWatchlist = $event" 
      :watchlist="watchlist" 
      @updateWatchlist="watchlist = $event"
      @modalId="showMovieId = $event"
      />
    </transition>

    <Nav :watchlist='isWatchlist' @changeWatchlist="isWatchlist = $event"/>
    <Header :randomMovie="randomMovie" @input="searchInput = $event" @showMovieId="showMovieId = $event"/>
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

export default {
  name: 'app',
  components: {
    Nav,
    Header,
    TrendingWeek,
    Footer,
    Watchlist,
    Modal,
  },
  data() {
    return {
      isWatchlist: false,
      randomMovie: {},
      trendingMovies: [],
      isModal: false,
      showMovie: {},
      showMovieId: '',
      watchlist: []
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
      localStorage.setItem('watchlist', JSON.stringify(this.watchlist));
    },
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
  created(){
    this.watchlist = JSON.parse(localStorage.getItem('watchlist'));
    if(this.watchlist === null) this.watchlist = [];
  }
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
