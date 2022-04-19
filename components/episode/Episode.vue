<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>

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
      error: false,
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