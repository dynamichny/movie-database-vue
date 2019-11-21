<template>
  <div class="search-outputs">
    <div class="output" v-for="movie in movies" :key="movie.id" @click="$parent.$emit('showMovieId', movie.id)">
      <img v-if="movie.poster_path" :src="'https://image.tmdb.org/t/p/w200'+movie.poster_path"/>
      <p class="output-title">{{movie.title}} ({{movie.release_date.split('-')[0]}})</p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'SearchOutputs',
  props: ['movies'],
};
</script>

<style scoped lang='sass'>
.search-outputs
  z-index: 9999
  max-width: 1000px
  position: absolute
  top: calc(20% + 60px)
  left: 0
  right: 0
  width: 60%
  height: 50vh
  margin: 0 auto
  overflow-y: auto

.output
  box-sizing: border-box
  display: flex
  align-items: center
  background: rgb(36,36,36)
  padding: 10px 20px 0 20px 
  width: 100%
  min-height: 100px
  border: 1px solid rgba(0,0,0,0.2)
  cursor: pointer
  &:hover
    background: rgb(45,45,45)
  img
    height: 80px

.output-title
  color: white
  margin: 0 30px
  font-size: 2.5rem
  padding: 7px 0


.output-title
  text-decoration: underline #01d277

@media (max-width: 550px)
  .search-outputs
    width: 85%
  .output-title
    font-size: 2rem

</style>