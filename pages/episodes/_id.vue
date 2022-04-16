<template>
  <div>
    <!-- {{episodes}} -->
    <div v-for="item in episodes" :key="item.id">
      <!-- <n-link :to="'/episodes/' + substr(item.url)"> -->
        {{ item.name }}
        {{ item.episode }}
        <!-- {{item.url}} -->
        <!-- <Characters :idCharacter="substr(item)" /> -->
      <!-- </n-link> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      episodes: [],
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
      this.episodes = datas.data;
    console.log(this.episodes);

    }
    // if(datas.data)
    // console.log(this.created);
    // console.log(datas.data.info.next);
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