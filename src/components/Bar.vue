<template>
  <div>
    <a @click.prevent="$emit('registerClicked', true)" v-if="!user">Register</a>
    <a @click.prevent="$emit('loginClicked', true)" v-if="!user">Log in</a>
    <a v-if="user">{{user.email}}</a>
    <a @click.prevent="$emit('logout', true)" v-if="user">Log out</a>
    
  </div>
</template>

<script>
export default {
  name: 'Bar',
  props: ['user']
};
</script>

<style scoped lang='scss'>
  div{
    background: black;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-end;
    margin: 0 auto;
  }
  a{
    color: white;
    text-decoration: none;
    padding: 5px;
    font-size: 1.5rem;
    cursor: pointer;
    transition: color 0.3s;
    margin: 0 20px;
    &:hover{
      color: #01d277;
    }
  }
</style>