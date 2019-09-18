<template>
  <div class="trendingWeekWrapper">
    <h2>Trending this week</h2>
    <div class="movieList">
      <MoviePoster v-for="movie in trendingMovies" :key="movie.id" :movie="movie" @click.native="showModal(movie.id)" />
    </div>
  </div>
</template>

<script>
import MoviePoster from './MoviePoster.vue'

export default {
  name: 'TrendingWeek',
  components: {
    MoviePoster,
  },
  data() {
    return {
      getMovieid: '',
    }
  },
  props: [
    'trendingMovies'
  ],
  methods: {
    showModal(id){
      this.$emit('movieid', id)
    },
  },
  computed: {
    setMovieid(){
      this.$emit('movieid', this.getMovieid);
    },
  },
};
</script>

<style scoped lang='scss'>
h2{
  font-size: 3rem;
  font-weight: 400;
  margin: 60px 22% 0;
  color: white;
  text-align: center;
}
.movieList{
  margin: 30px 22% 0;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  grid-gap: 50px 20px;
  @media (max-width: 757px){
    margin: 30px 20px 0;
  }
}
</style>