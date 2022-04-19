<template>
  <div class="episode text-white font-bold py-2 px-4 rounded">
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
    await axios
      .get("https://rickandmortyapi.com/api/episode/" + this.episodeId)
      .then((res) => {
        this.episode = res.data;
      })
      .catch((error) => {
        this.error = true;
      });
  },
};
</script>