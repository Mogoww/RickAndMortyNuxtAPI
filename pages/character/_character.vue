

<template>
  <div>
    <div v-for="item in characters" :key="item.id">
      <n-link :to="'character/person/'+item.id"
        >
        <Character :character="item" />
      </n-link>
    </div>
  </div>
</template>

<style>
</style>

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
    console.log(datas.data.results[0]);
    this.characters = datas.data.results;
  },
};
</script>