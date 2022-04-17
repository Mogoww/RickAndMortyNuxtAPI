<template>
  <div>
    <!-- List épisodes -->
    <div v-if="episodes">
      <div v-for="item in episodes" :key="item.id">
        <n-link :to="'/episodes/' + substr(item.url)">
          {{ item.name }}
          {{ item.episode }}
        </n-link>
      </div>
    </div>

    <!-- Juste un épisode -->
    <div v-if="episode" style="margin: 100px">
      <!-- {{ episode.name }}
      {{ episode.episode }} -->
      <Episode :episode="episode"/>
      <!-- Carousel des characters présent dans l'épisode -->
      <Carousel :data="episode.characters" :type="'characters'" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      episodes: null,
      episode: null,
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas;
    if (id) {
      datas = await axios.get("https://rickandmortyapi.com/api/episode/" + id);
      this.episode = datas.data;
    } else {
      datas = await axios.get("https://rickandmortyapi.com/api/episode");
      this.episodes = datas.data.results;
    }
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script>

<style  lang='scss' >
.card-margin {
  margin: 10px 30px 10px 30px;
}

button.slick-prev:before,
button.slick-next:before {
  color: black !important;
}
</style>