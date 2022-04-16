<template>
  <div>
    <div v-if="characters">
      <div class="container-character">
        <div class="card" v-for="item in characters" :key="item.id">
          <n-link :to="'/characters/' + item.id">
            <CardCharacter :idCharacter="item.id" />
          </n-link>
        </div>
      </div>
    </div>

    <div v-if="character">
      <!-- {{char}} -->
      <Character :idCharacter="character.id" />
      <div class="episode" v-for="item in character.episode" :key="item.id">
        <n-link :to="'/episodes/' + substr(item)">
          <Episode :episode="item" />
        </n-link>
      </div>
      <!--     
    <Character :character="character" />

    <div class="container-episode">
      <div class="episode" v-for="item in character.episode" :key="item.id">
        <n-link :to="'/episodes/' + substr(item)">
          <Episode :episode="item" />
        </n-link>
      </div>
    </div> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      characters: null,
      character: null,
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas;

    if (id) {
      datas = await axios.get(
        "https://rickandmortyapi.com/api/character/" + id
      );
      this.character = datas.data;
    } else {
      datas = await axios.get("https://rickandmortyapi.com/api/character");
      this.characters = datas.data.results;
    }
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
.container-character {
  display: flex;
  flex-wrap: wrap;
  .card {
    padding: 10px;
  }
}
</style>