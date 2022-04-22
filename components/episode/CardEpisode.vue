<template>
  <div class="episode text-white font-bold py-2 px-4 rounded">
    <div>
      {{ episode.name }}
    </div>
    <div>
      {{ episode.episode }}
    </div>
  </div>
</template>


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

<style  scoped>
.episode {
  background: linear-gradient(to bottom right, #9bca3d, #2e8841);
  margin: 10px;
  text-align: center;
}
</style>