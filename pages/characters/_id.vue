<template>
  <div>
    <!-- Loading  -->
    <loading v-show="loadingStatus" />

    <!-- list characters -->
    <div v-if="characters">
      <div class="container-character">
        <div class="card" v-for="item in characters" :key="item.id">
          <n-link :to="'/characters/' + item.id">
            <CardCharacter :idCharacter="item.id" />
          </n-link>
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
  </div>
</template>

<script>
import axios from "axios";
import Loading from "~/components/Loading";
export default {
  components: {
    Loading,
  },
  data() {
    return {
      characters: null,
      characterId: null,
      page: 1,
      pageMax: null,
      error: false,
      loadingStatus: true,
    };
  },
  created: async function (t) {
    let id = this.$route.params.id;
    if (id) {
      this.characterId = id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) {
        this.page = this.$route.query.page;
      }

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
  },
};
</script>

<style lang="scss" scoped>
@import "~/assets/scss/variables";
.container-character {
  display: flex;
  flex-wrap: wrap;
  .card {
    padding: 10px;
  }
}
</style>
