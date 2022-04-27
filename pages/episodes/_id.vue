<template>
  <div>
    <!-- Loading  -->
    <loading v-show="loadingStatus" />

    <!-- List épisodes -->
    <div v-if="episodes">
      <div v-for="item in episodes" :key="item.id" class="relative">
        <n-link :to="'/episodes/' + substr(item.url)">
          <CardEpisode :episodeId="substr(item.url)" />
        </n-link>
        <Like
          :id="'e' + substr(item.url)"
          :type="'episode'"
          :url="item.url"
        />
      </div>
      <Pagination :pageNum="page" :pageMax="pageMax" />
    </div>

    <!-- Juste un épisode -->
    <div v-if="episodeId" style="margin: 100px">
      <Episode :episodeId="episodeId" />
    </div>

    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
  </div>
</template>

<script>
import Episode from "~/components/episode/Episode.vue";
import CardEpisode from "~/components/episode/CardEpisode.vue";
import axios from "axios";

export default {
  components: { Episode, CardEpisode },
  data() {
    return {
      episodes: null,
      episodeId: null,
      page: 1,
      pageMax: null,
      error: false,
      loadingStatus: true,
    };
  },
  created: async function () {
    if (this.$route.params.id) {
      this.episodeId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;
      await axios
        .get("https://rickandmortyapi.com/api/episode/?page=" + this.page)
        .then((res) => {
          this.episodes = res.data.results;
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

<style  lang='scss' >
</style>