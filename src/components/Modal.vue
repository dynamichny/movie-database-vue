<template>
  <div class='movie-popup'>
    <div class="bg-popup" :style='bg'></div>
      <div class="movie-cont">
        <div class="movie-more">
          <img :src="posterPath" alt="Here should be a poster." />
          <div class="descrpition">
            <div class="description-heading">
              <button class="close close-more" @click="closeModal()"><div></div></button>
              <p class="title">{{movie.title}}</p>
              <div class="generes-length">
                <p class="generes"></p>
                <div class="movie-length">
                  <p>{{movie.runtime}} min</p>
                </div>
              </div>
              <div class="line"></div>
              <div class="info">
                <div class="rating">
                  <p class="rating1"><b>{{movie.vote_average}}</b>/10</p>
                  <p class="rating2">{{movie.vote_count}} votes</p>
                </div>
                <div class="release">
                  <p class="release1">{{movie.release_date}}</p>
                  <p class="release2">release date</p>
                </div>
              </div>
            </div>
            <div class="description-text">{{movie.overview}}</div>
            <div class="buttons">
              <div class="btn-seemore"><a :href="imdbLink" target='_blank'>See more</a></div>
              <div class="btn-towatchlist"  @click="handleWatchlist()">{{watchlistText}}</div>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: ['movie', 'watchlist'],
  methods: {
    closeModal(){
      this.$emit('closeModal', false);
      this.$emit('resetModalId', '');
    },
    handleWatchlist(){
      let wl = [...this.watchlist];
      if(wl.filter(mv => mv.title == this.movie.title).length > 0){
        wl.splice(wl.findIndex(mv => mv.title == this.movie.title), 1)
      } else {
        wl.push(this.movie)
      }
      this.$emit('updateWatchlist', wl)
    }
  },
  computed: {
    imdbLink(){
      return `https://www.imdb.com/title/${this.movie.imdb_id}`;
    },
    posterPath(){
      return `https://image.tmdb.org/t/p/w500${this.movie.poster_path}`;
    },
    bg(){
      return {
        background: `url("https://image.tmdb.org/t/p/w500${this.movie.backdrop_path}")`, 
        backgroundSize: 'cover', 
        backgroundPosition: 'center'
      }
    },
    watchlistText(){
      if(this.watchlist.filter(mv => mv.title == this.movie.title).length > 0){
        return "Remove from watchlist"
      } else {
        return "Add to watchlist"
      }
    }
  },
  mounted(){
    window.addEventListener('keydown', (e) => {
      if(e.keyCode === 27) this.closeModal();
    });
  }
};
</script>

<style scoped lang='scss'>
.movie-popup {
  position: fixed;
  background: rgba(0, 0, 0, 0.6);
  z-index: 99999999;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0; }

.bg-popup {
  visibility: hidden;
  background: #181818;
  background-size: cover;
  background-position: center;
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100vw;
  height: 100%;
  z-index: -1; }

.movie-cont {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center; }

.movie-more {
  width: 1120px;
  height: 700px;
  margin: 20px;
  background: white;
  position: relative;
  border-radius: 8px;
  display: flex; }
  .movie-more img {
    height: 100%;
    border-radius: 8px 0 0 8px; }

.descrpition {
  position: relative; }

.description-heading {
  background: white;
  position: absolute;
  width: 100%;
  height: 300px;
  top: 5%;
  right: 5%;
  box-shadow: 0px 0px 30px -10px rgba(0, 0, 0, 0.75);
  overflow-y: auto; }
  .title {
    font-size: 3.7rem;
    line-height: 0.9;
    color: black;
    margin: 30px 30px 0 30px;
    @media (max-width: 800px){
      font-size: 3.2rem;
    }
  }

.generes-length {
  display: flex;
  justify-content: space-between;
  margin: 10px 30px 20px; }

.generes {
  font-size: 1.5rem;
  color: #01d277; }

.movie-length {
  background: rgba(112, 112, 112, 0.2);
  border-radius: 17px;
  display: flex;
  justify-content: center;
  align-items: center; }
  .movie-length p {
    margin: 0;
    text-align: center;
    padding: 0 10px;
    font-size: 1.5rem; }

.line {
  background: rgba(0, 0, 0, 0.2);
  height: 1px;
  width: 95%;
  margin: 0 auto; }

.info {
  width: 100%;
  margin: 20px 0;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  text-align: center;
  align-items: center; }

.rating, .release {
  margin: 20px;
  @media (max-width: 550px){
    margin: 0;
  }
   }

.rating1, .release1 {
  font-size: 3.0rem;
  color: black;
  margin: 0; }

.rating2, .release2 {
  margin: 0;
  font-size: 1.3rem; }

.description-text {
  margin: 350px 25px 25px;
  height: 245px;
  font-size: 2.0rem;
  overflow-y: auto; }

.buttons {
  position: absolute;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 90px; }

.btn-seemore {
  background: #01d277; }
  .btn-seemore a {
    color: white; }

.btn-towatchlist {
  background: #313131;
  border-radius: 0 0 8px 0; }

.btn-towatchlist, .btn-seemore {
  text-align: center;
  font-size: 2.4rem;
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
  cursor: pointer;
  opacity: 0.98; }
  .btn-towatchlist:hover, .btn-seemore:hover {
    opacity: 1; }

.close {
  position: absolute;
  top: 5px;
  right: 0;
  border: none;
  background: none;
  padding: 15px;
  cursor: pointer;
  margin: 0; }
  .close div {
    border-radius: 5px;
    transform: rotate(45deg);
    width: 20px;
    height: 2px;
    background: black;
    position: relative; }
    .close div::after {
      border-radius: 5px;
      content: '';
      position: absolute;
      width: 20px;
      height: 2px;
      background: black;
      top: 0;
      left: 0;
      transform: rotate(90deg); }

@media (max-width: 750px) {
  html {
    font-size: 45%; }
  .movie-more {
    height: 450px; }
  .description-heading {
    height: 200px; }
  .description-text {
    margin: 230px 25px 25px;
    height: 150px; }
  .buttons {
    height: 60px; } }

@media (max-width: 550px) {
  .movie-popup {
    height: 100vh;
    margin: 0;
    background: black;
    text-shadow: 1px 1px 2px black; }
  .bg-popup {
    visibility: visible;
    filter: blur(20px);
    transition: filter 0.5s ease-in-out;
    display: flex; }
  .close {
    top: 15px;
    right: 15px; }
    .close div {
      width: 30px; }
      .close div:after {
        width: 30px; }
  .descrpition {
    background: none;
    margin: 43vh 0 0;
    align-self: flex-end;
    position: static; }
  .description-heading {
    position: static;
    background: 0;
    box-shadow: none;
    margin: 0;
    height: auto; }
    .description-heading .title {
      color: white;
      font-weight: bold;
      margin: 10px;
      font-size: 2.5rem; }
    .description-heading .generes-length {
      margin: 0 5% 5px; }
      .description-heading .generes-length .movie-length {
        background: rgba(0, 0, 0, 0.4);
        color: white; }
    .description-heading .info {
      margin: 0 0 15px;
      flex-wrap: nowrap;
      color: white; }
    .description-heading .rating1, .description-heading .release1 {
      font-size: 2.2rem;
      color: white; }
    .description-heading .rating, .description-heading .release2 {
      margin-top: 5px;
      margin-bottom: 0px; }
  .description-text {
    color: white;
    margin: 0 20px;
    height: 20vh;
    box-sizing: border-box;
     }
  .movie-cont {
    align-items: center; }
  .movie-more {
    flex-direction: column;
    align-items: flex-end;
    background: 0;
    margin: auto;
    padding: 0 0 100px;
    box-sizing: border-box;
    height: 100vh;
    overflow-y: hidden; }
    .movie-more img {
      position: absolute;
      top: 5px;
      left: 0;
      right: 0;
      border-radius: 8px;
      height: 40vh;
      margin: 5px auto;
      box-shadow: 0px 5px 30px 5px rgba(0, 0, 0, 0.75); }
  .buttons {
    bottom: 2%; }
  .btn-seemore, .btn-towatchlist {
    margin: 10px;
    background: rgba(0, 0, 0, 0.4);
    border-radius: 15px;
    font-size: 2.2rem; } }
</style>