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

    <div v-if="characterId">
      <Character :idCharacter="characterId" />
      <!-- <div class="episode" v-for="item in character.episode" :key="item.id">
        <n-link :to="'/episodes/' + substr(item)">
          <CardEpisode :episode="item" />
        </n-link>
      </div> -->
    </div>
    <div v-if="error">
      <NotFoundPage/>
    </div>

  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      characters: null,
      characterId: null,
      error: false,
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas;
    if (id) {
      this.characterId = id;
    } else {
      let page = 1;
      if (this.$route.query.page) {
        page = this.$route.query.page;
      }

      await axios
        .get("https://rickandmortyapi.com/api/character/?page=" + page)
        .then((res) => {
          this.characters = res.data.results;
        })
        .catch((error) => {
          this.error = true
        });
      // if (datas) 
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