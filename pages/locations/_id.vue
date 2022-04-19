<template>
  <div>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      locations: null,
      locationId: null,
    };
  },
  created: async function () {
    // let id = this.$route.params.id;
    // let datas;
    // if (id) {
    //   this.episodeId = id;
    // } else {
    //   datas = await axios.get("https://rickandmortyapi.com/api/episode");
    //   this.episodes = datas.data.results;
    // }
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