<template>
  <input type="text" v-model="search" placeholder="Seach a movie...">
</template>

<script>

export default {
  name: 'SearchInput',
  data(){
    return{
      search: '',
    }
  },
  watch: {
    search: function(){
      this.$emit('search', this.search)
    }
  }
};
</script>

<style scoped lang='scss'>
  input{
    width: 60%;
    height: 60px;
    max-width: 1000px;
    box-sizing: border-box;
    border: none;
    border-bottom: 2px solid #01d277;
    background: #181818;
    position: absolute;
    top: 20%;
    left: 0;
    right: 0;
    margin: 0 auto;
    color: white;
    font-size: 2.2rem;
    padding: 15px;
    transition: all .2s;
    outline: none;
    font-family: 'Source Code Pro', monospace;
    @media (max-width:550px){
      width: 85%;
    }
    &:focus {
      border-bottom: 5px solid #01d277;
      box-shadow: 0px 10px 15px -8px #01d277;
    }
    &::placeholder{
      font-family: 'Source Code Pro', monospace;
      font-size: 1.8rem
    }
  }
</style>