<template>
  <div>
    {{episodeData.episode}}

    {{episodeData.name}}
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  props: ["episode"],
  data() {
    return {
      episodeData: [],
    };
  },
  created: async function () {
    let datas = await axios.get(
      this.episode
    );
    this.episodeData = datas.data;
  },
};
</script>