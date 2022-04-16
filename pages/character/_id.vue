<template>
  <div>
    <Person :character="character" />

    <div class="container-episode">
      <div class="episode" v-for="item in character.episode" :key="item.id">
        <n-link :to="'/episodes/' + substr(item)">
          <Episode :episode="item" />
        </n-link>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      character: [],
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas = await axios.get(
      "https://rickandmortyapi.com/api/character/" + id
    );
    this.character = datas.data;
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
      // this.substr = this.str.substr(1, 4);
    },
  },
};
</script>

<style  lang='scss' scoped>
@import "~/assets/scss/variables";
.container-episode {
  display: flex;
  flex-wrap: wrap;
  .episode {
    padding: 10px;
    border: solid;
  }
}
</style>