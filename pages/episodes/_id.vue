<template>
  <div>
    <!-- {{episodes}} -->
    <div v-if="episodes">
      <div v-for="item in episodes" :key="item.id">
        <n-link :to="'/episodes/' + substr(item.url)">
          {{ item.name }}
          {{ item.episode }}
          <!-- {{item.url}} -->
          <!-- <Characters :idCharacter="substr(item.url)" /> -->
        </n-link>
      </div>
    </div>

    <div v-if="episode">
      {{ episode.name }}
      {{ episode.episode }}
      <div v-for="item in episode.characters" :key="item.id">
        <Person :idCharacter="substr(item)" />
      </div>
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
    if (id == null) {
      datas = await axios.get("https://rickandmortyapi.com/api/episode");
      this.episodes = datas.data.results;
    } else {
      datas = await axios.get("https://rickandmortyapi.com/api/episode/" + id);
      this.episode = datas.data;
    }
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script>

<style  lang='scss' scoped>
</style>