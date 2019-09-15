<template>
  <div class='footerWrapper'>
    <p>
      Powered by
      <a href="https://www.themoviedb.org/">themoviedb.org</a>
      API.
    </p>
  </div>
</template>

<script>
export default {
  name: 'Footer',
};
</script>

<style scoped lang='scss'>
.footerWrapper {
  margin: 80px 0 0;
  width: 100%;
  height: 70px;
  border-top: 1px solid #01d277;
  background: #292929;
  font-size: 2.2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}
a {
  color: #01d277;
  text-decoration: none;
}

</style>