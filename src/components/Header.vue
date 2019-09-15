<template>
  <div class="headerWrapper">
    <HeaderBackground :src="bgImage"  />
    <SearchInput @search="searchIn = $event" />
    <SearchOutputs v-if="searchOut" :movies="searchOut" @movieid="showMovieId = $event" />
    <h2>{{randomMovie.title}}</h2>
  </div>
</template>

<script>
import HeaderBackground from './HeaderBackground.vue'
import SearchInput from './SearchInput.vue'
import SearchOutputs from './SearchOutputs.vue';
import debounce from 'lodash.debounce';


export default {
  name: 'Header',
  components: {
    HeaderBackground,
    SearchInput,
    SearchOutputs

  },
  props: {
    randomMovie: Object
  },
  data(){
    return{
      searchIn: '',
      searchOut: '',
      showMovieId: '',
    }
  },
  watch: {
    searchIn: debounce(function(){
      if(this.searchIn != ''){
        fetch(`https://api.themoviedb.org/3/search/movie?api_key=485dd1f1ee71083619712efed20ee4bb&language=en-US&query=${this.searchIn}&page=1&include_adult=false`)
          .then(resp => resp.json())
          .then(resp => {
            this.searchOut = resp.results;
          });
      } else {
        this.searchOut = ''; 
      }
    },300),
    showMovieId: function(){
      this.$emit('showMovieId', this.showMovieId)
    }
  },
  computed: {
    bgImage(){
      return `https://image.tmdb.org/t/p/original${this.randomMovie.backdrop_path}`;
    }
  },
};
</script>

<style scoped lang='scss'>
  .headerWrapper{
    width: 100%;
    height: 450px;
    position: relative;
  }
  h2{
    color: white;
    position: absolute;
    bottom: 10%;
    left: 10%;
    font-size: 3rem;

  }
</style>