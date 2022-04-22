<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    
    <div>
      <div v-if="episode.episode != null">
        <img
          :src="
            require('~/assets/images/' + substrSaison(episode.episode) + '.jpg')
          "
        />
      </div>
      components
      {{ episode.name }}<br />
      {{ episode.air_date }}<br />
      {{ episode.episode }}<br />

      <!-- Carousel des characters présent dans l'épisode -->
      <Carousel :data="episode.characters" :type="'characters'" />
    </div>
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
  methods: {
    substrSaison: function (data) {
      if (data != null) return data.substring(0, data.indexOf("E"));
    },
  },
};
</script>