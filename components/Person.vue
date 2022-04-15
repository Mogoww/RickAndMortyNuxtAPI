<template>
  <div>
    <img :src="character.image" :alt="character.name" style="width: 50%" />
    {{ character.name }}
  </div>
</template>



<script>
import axios from "axios";
export default {
  props: ["character"],
};
</script>

<style lang='scss' scoped>
@import "~/assets/scss/variables";
</style>