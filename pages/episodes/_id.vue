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
    <div v-if="episodeId" style="margin: 100px">
      <Episode :episodeId="episodeId" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      episodes: null,
      episodeId: null,
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas;
    if (id) {
      this.episodeId = id;
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
</style>