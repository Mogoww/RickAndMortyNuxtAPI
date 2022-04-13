<template>
  <div>
    <img :src="character.image" />
    <p>
      {{ character.name }}
    </p>
  </div>
</template>



<script>
import axios from "axios";
export default {
  props: ["character"],
  //   created: async function () {
  //     let datas = await axios.get(
  //       "https://rickandmortyapi.com/api/character/" + this.character.id
  //     );
  //     console.log(datas.data.results);
  //     // this.characters = datas.data.results;
  //   },
};
</script>

<style lang='scss' scoped>
@import "~/assets/scss/variables";
</style>