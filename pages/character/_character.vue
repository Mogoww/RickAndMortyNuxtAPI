

<template>
  <div class="container-character">
    <div class="card" v-for="item in characters" :key="item.id">
      <n-link :to="'character/person/' + item.id">
        <Characters :idCharacter="item.id" />
      </n-link>
    </div>
    
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      characters: [],
    };
  },
  created: async function () {
    let datas = await axios.get("https://rickandmortyapi.com/api/character");
    this.characters = datas.data.results;
    console.log(datas.data.info.pages);
  },
};
</script>

<style  lang='scss' scoped>
@import "~/assets/scss/variables";
.container-character {
  display: flex;
  flex-wrap: wrap;
  .card {
    padding: 10px;
  }
}
</style>