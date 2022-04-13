<template>
  <div>
      HOME
  </div>
</template>

<style>

</style>

<script>
import axios from "axios";
export default {
  data() {
    return {
      character: [],
    };
  },
  created: async function () {
    let datas = await axios.get("https://rickandmortyapi.com/api/character");
    console.log(datas.data.results);
    this.character = datas.data;
  },
};
</script>