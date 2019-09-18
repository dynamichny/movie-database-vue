<template>
  <div class="buttonWrapper">
    <div class="outter">
      <div class="inner">see more -></div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'HeaderButton',
};
</script>

<style scoped lang='scss'>
.buttonWrapper{
  width: 200px;
  height: auto;
  margin: 0;
  position: absolute;
  bottom: 3%;
  left: 10%
}
.outter{
  height: 100%;
  width: 100%;
  background: black;
  color: white;
  display: flex;
  align-items: center;
  padding: 10px;
  box-sizing: border-box;
  cursor: pointer;
  &:hover{
    .inner{
      transform: translateX(60px)
    }
  }
}
.inner{
  transition: all 0.2s ease-in-out;
  font-size: 1.8rem;
}
</style>