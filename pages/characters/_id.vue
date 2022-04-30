<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    <div v-else>
      <!-- list characters -->
      <div v-if="characters">
        <!-- Loading  -->
        <Loading v-show="loadingStatus" />

        <Recherche :typeRecherche="'characters'" />

        <div class="container-character flex flex-wrap items-center justify-center">
          <div class="card relative" v-for="item in characters" :key="item.id">
            <n-link :to="'/characters/' + item.id">
              <CardCharacter :idCharacter="item.id" />
            </n-link>
            <Like :id="'c' + item.id" :type="'character'" :url="item.url" />
          </div>
        </div>
        <Pagination :pageNum="page" :pageMax="pageMax" />
      </div>
      <!-- One character -->
      <div v-if="characterId">
        <Character :idCharacter="characterId" />
      </div>
    </div>
  </div>
</template>

<script>
import Character from "~/components/character/Character.vue";
import CardCharacter from "~/components/character/CardCharacter.vue";
import Loading from "~/components/Loading.vue";
import Like from "~/components/Like.vue";

import axios from "axios";
export default {
  components: {
    Loading,
    CardCharacter,
    Character,
    Like,
  },
  data() {
    return {
      characters: null,
      characterId: null,
      page: 1,
      pageMax: null,
      error: false,
      loadingStatus: true,
      params: null,
    };
  },
  created: async function () {
    if (this.$route.params.id) {
      this.characterId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;
      this.createUrl();
      await axios
        .get("https://rickandmortyapi.com/api/character/?page=" + this.page + this.params)
        .then((res) => {
          this.characters = res.data.results;
          this.pageMax = res.data.info.pages;
          this.loadingStatus = false;
        })
        .catch((error) => {
          this.error = true;
          this.loadingStatus = false;
        });
    }
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
    createUrl() {
      let urlPara = this.$route.query;
      let paramTempo = "";
      Object.keys(urlPara).map(function (key) {
        if (key != "page" && (key == "name" || key == "status" || key == "gender"))
          paramTempo += key + "=" + urlPara[key];
        if (key != Object.keys(urlPara).pop()) paramTempo += "&";
      });
      this.params = paramTempo;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "~/assets/scss/variables";
.container-character {
  .card {
    padding: 10px;
  }
}
</style>
