<template>
  <div>
    {{ episode.episode }}
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  props: ["episodeId"],
  data() {
    return {
      episode: [],
    };
  },
  created: async function () {
    let datas = await axios.get(
      "https://rickandmortyapi.com/api/episode/" + this.episodeId
    );
    this.episode = datas.data;
  },
};
</script>