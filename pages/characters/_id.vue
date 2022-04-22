<template>
  <div>
    <!-- Loading  -->
    <Loading v-show="loadingStatus" />

    <!-- list characters -->
    <div v-if="characters">
      <div
        class="container-character flex flex-wrap items-center justify-center"
      >
        <div class="card relative" v-for="item in characters" :key="item.id">
          <n-link :to="'/characters/' + item.id">
            <CardCharacter :idCharacter="item.id" />
          </n-link>
          <Like
            :id="'c' + item.id"
            :type="'character'"
            :url="item.url"
            class="absolute like"
          />
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
    };
  },
  created: async function () {
    // this.$auth.$storage.setUniversal("key", "val")
    if (this.$route.params.id) {
      this.characterId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;

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
  .card {
    padding: 10px;
  }
}
.like {
  right: 18px;
  bottom: 15px;
}

@media only screen and (max-width: 800px) {
.like {
  right: 18px;
  bottom: 15px;
}
}
</style>
