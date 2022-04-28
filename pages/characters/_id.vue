<template>
  <div>
    <!-- Loading  -->
    <Loading v-show="loadingStatus" />

    <Recherche :parentData="parentData" @interface="handleFcAfterDateBack" />

    <!-- list characters -->
    <div v-if="characters">
      <div
        class="container-character flex flex-wrap items-center justify-center"
      >
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

    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    sqdsd
    {{ parentData }}
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
      parentData: [],
      withData: null,
    };
  },
  created: async function () {
    if (this.$route.params.id) {
      this.characterId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;
      console.log(this.withData);
      await axios
        .get("https://rickandmortyapi.com/api/character/?page=" + this.page)
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
    handleFcAfterDateBack(event) {
      this.characters = event.results;
      this.pageMax = event.info.pages;
      if (event.info.next)
        this.withData = event.info.next.substring(event.info.next.indexOf("&"));
      this.$router.push({ path: 'characters', query: { name: 'private' }})

      // https://rickandmortyapi.com/api/character/?page=2&name=rick
      // this.loadingStatus = false;
      console.log("data after child handle: ", event); // get the data after child dealing
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
