<template>
  <div class="registerWrapper">
    <h2>Log in</h2>
    <button class="close" @click.prevent="$emit('closeLogin', false)">Close</button>
    <form @submit.prevent="$emit('login', {email, password})">
      <div class="inputs">
        <div class="email">
          <label for="email">Email</label>
          <input type="email" name="email" v-model="email" required>
        </div>
        <div class="password">
          <label for="password">Password</label>
          <input type="password" name="password" v-model="password" required>
        </div>
      </div>
      <button class="submit" type="submit">Log in</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data(){
    return{
      email: '',
      password: ''
    }
  }
};
</script>

<style scoped lang='scss'>
  .registerWrapper{
    width: 600px;
    height: 400px;
    position: fixed;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    margin: auto;
    background: #101010;
    color: white;
    z-index: 10000;
    font-size: 2rem;
    @media (max-width: 600px){
      width: 100%;
      height: 100%;
    }
  }
  h2{
    text-align: center;
    margin: 25px 0 0;
  }
  .close{
    position: absolute;
    top: 10px;
    right: 10px;
    color: white;
    background: none;
    border: none;
    font-family: 'Source Code Pro', monospace;
    cursor: pointer;
  }
  form{
    height: 80%;;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
  }
  .inputs{
    height: 40%;
    width: 100%;;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    div{
      margin: auto;
      width: 75%;
      display: flex;
      justify-content: space-between;
      align-items: center;
      @media (max-width: 600px){
        flex-direction: column;
      }
      input{
        background: none;
        border: none;
        border-bottom: 2px solid #01d277;
        padding: 15px;
        outline: none;
        color: white;
        width: 50%;
        transition: all 0.15s;
        @media (max-width: 600px){
          width: 80%;
          margin: 10px;
        }
        &:focus {
          border-bottom: 5px solid #01d277;
          box-shadow: 0px 10px 15px -15px #01d277;
        }
      }
    }
  }
  .submit{
    width: 130px;
    height: 50px;
    background: #01d277;
    color: white;
    border: none;
    font-size: 2rem;
    font-family: 'Source Code Pro', monospace;
    font-weight: bold;
    cursor: pointer;
    position: relative;
    transition: transform 0.3s;
    outline: none;
    &::after{
      outline: none;
      z-index: -1;
      content: '';
      width: 100%;
      height: 100%;
      position: absolute;
      top: 10px;
      right: 10px;
      background: #181818;
      transition: transform 0.15s;

    }
    &:active{
      transform: translateX(-10px) translateY(10px);
      &::after{
        transform: translateX(10px) translateY(-10px);
      }
    }
  }
</style>