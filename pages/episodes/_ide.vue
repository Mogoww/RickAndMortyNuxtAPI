<template>
  <div>
    <!-- {{ episode }}

    <div class="container-character">
      <div v-for="item in episode.characters" :key="item.id">
        <n-link :to="'/character/person/' + substr(item)">
          <Characters :idCharacter="substr(item)" />
        </n-link>
      </div>
    </div> -->
    sdfsdf
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      episode: [],
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas = await axios.get(
      "https://rickandmortyapi.com/api/episode/" + id
    );
    this.episode = datas.data;
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script>

<style  lang='scss' scoped>
@import "~/assets/scss/variables";
.container-character {
  display: flex;
  flex-wrap: wrap;
  .card {
    padding: 10px;
  }
}
</style>