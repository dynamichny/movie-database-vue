<template>
  <div class="watchlistWrapper">
    <button class="close-watchlist" @click="closeWatchlist">
      <div></div>
    </button>
    <h2>Your watchlist:</h2>
    <div class="movies">
      <div v-for="movie in watchlist" :key="movie.id" class="watchlist-movie" @click="showModal(movie.id, $event)">
          <button class="close-watchlist" @click="removeFromWatchlist(movie)"><div class="arrow"></div></button>
          <img :src="'https://image.tmdb.org/t/p/w200' + movie.poster_path" />
          <p class="title">{{movie.title}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Watchlist',
  props: ['watchlist'],
  methods: {
    closeWatchlist(){
      this.$emit('closeWl', false)
    },
    removeFromWatchlist(movie){
      let wl = [...this.watchlist];
      wl.splice(wl.findIndex(el => el.id == movie.id), 1)
      this.$emit('updateWatchlist', wl)
    },
    showModal(id, target){
      if(target.target.className !== 'arrow' && target.target.localName !== 'button'){
        this.$emit('modalId', id)
      }
    }
  }
};
</script>

<style scoped lang='sass'>
.watchlistWrapper
  transition: transform 0.17s ease-in-out
  position: fixed
  top: 0
  bottom: 0
  right: 0
  left: 0
  width: 100%
  height: 100%
  background: #181818
  z-index: 999999
  overflow-y: auto
  padding: 6vh 0 
  box-sizing: border-box
.close-watchlist
  position: absolute
  top: 10px
  right: 0
  border: none
  background: none
  padding: 15px
  margin: 0
  outline: none
  cursor: pointer
  div
    transform: rotate(45deg)
    width: 20px
    height: 2px
    background: white
    position: relative
    &::after
      content: ''
      position: absolute
      width: 20px
      height: 2px
      background: white
      top: 9px
      left: 9px
      transform: rotate(90deg)
h2
  color: white
  font-size: 3rem
  text-decoration: underline #01d277
  text-align: center

.movies
  .watchlist-movie
    cursor: pointer
    position: relative
    background: rgb(45,45,45)
    box-sizing: border-box
    display: flex
    align-items: center
    margin: 10px auto
    padding: 0 20px 
    max-width: 1000px
    width: 60%
    height: 150px
    border-bottom: 1px solid #01d277
    @media (max-width: 800px)
      width: 80%
    @media (max-width: 500px)
      width: 90%
    img
      height: 80%
    .title
      color: white
      font-size: 3rem
      margin: 0 auto
      padding: 0 30px
      justify-self: center
      text-align: center
      @media (max-width: 800px)
       font-size: 2.5rem
      @media (max-width: 500px)
        font-size: 2.2rem
      @media (max-width: 350px)
        font-size: 1.8rem
    .close
      z-index: 100000000000
      div
        background: #01d277
        &::after
          background: #01d277
    
      

@media (max-width: 750px)
  .watchlist
    .movies
      .watchlist-movie
        width: 80%

@media (max-width: 500px)
  .watchlist
    .movies
      .watchlist-movie
        width: 95%
</style>