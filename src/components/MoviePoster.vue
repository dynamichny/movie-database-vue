<template>
  <div class="movie">
    <div class='poster'>
      <img :src="posterImg" alt='Poster' />
    </div>
    <div class="description">
      <p class="title">{{movie.title}}</p>
      <p class="realise_date">Release date: {{movie.release_date}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MoviePoster',
  props: ['movie'],
  computed: {
    posterImg(){
      return `https://image.tmdb.org/t/p/w500${this.movie.poster_path}`
    },
  },
};
</script>

<style scoped lang='scss'>
  .movie {
    background: white;
    cursor: pointer;
  }
  img {
    border: 1px solid white;
    box-sizing: border-box;
    width: 100%;
    height: 100%;
  }
  .description {
    background: white;
    width: 100%;
    height: auto;
  }
  .title {
    font-size: 2.4rem;
    margin: 0 5px;
    padding-top: 5px;
    line-height: 1.2;
    color: #181818;
  }
  .realise_date {
    color: #01d277;
    line-height: 1;
    margin: 0 5px;
    padding: 10px 0 10px;
    align-self: flex-end;
  }
</style>