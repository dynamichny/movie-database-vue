<template>
  <div :style="image" class="image"></div>
</template>

<script>
export default {
  name: 'HeaderBackground',
  props: {
    src: String,
  },
  computed: {
    image() {
      return {
        backgroundImage: `url(${this.src})`,
      }
    },
  },
};
</script>

<style scoped lang='scss'>
  .image {
    width: 100%;
    height: 100%;
    background-repeat: no-repeat;
    background-size: cover;
    background-position: 50% 35%;
    z-index: -1;
    position: absolute;
    top: 0;
    left: 0;
    filter: brightness(0.6);
  }
</style>