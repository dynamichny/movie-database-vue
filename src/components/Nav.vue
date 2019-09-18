<template>
  <div class="navWrapper"> 
    <h1>Movie Database</h1>
    <a @click.prevent="showWatchlist()">Watchlist</a>
  </div>
</template>

<script>
export default {
  name: 'Nav',
  props: {
    watchlist: {
      type: Boolean,
      default: false
    },
  },
  methods: {
    showWatchlist(){
      this.$emit('changeWatchlist', true);
    }
  }
};
</script>

<style scoped lang='scss'>
  .navWrapper {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
    flex-wrap: wrap;
    color: #01d277;
    widows: 100%;
    height: auto;
    font-size: 170%;
    padding: 10px 0;
  }
  h1{
    font-family: 'Sarina', monospace;
    cursor: pointer;
    margin: 0 15px;
    @media (max-width: 500px){
      text-align: center;
      margin: 10px 15px;
    }
  }
  a{
    cursor: pointer;
    margin: 0 15px;
  }
</style>