<template>
  <div>
    components
    {{ episode.name }}<br />
    {{ episode.air_date }}<br />
    {{ episode.episode }}<br />

    <!-- Carousel des characters présent dans l'épisode -->
    <Carousel :data="episode.characters" :type="'characters'" />
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