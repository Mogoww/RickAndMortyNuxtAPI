<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    <div v-else>
      <!-- List épisodes -->
      <div v-if="episodes">
        <!-- Loading  -->
        <Loading v-show="loadingStatus" />

        <Recherche :typeRecherche="'episode'" @interface="recherche" />

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
      <div v-if="episodeId">
        <Episode :episodeId="episodeId" />
      </div>
    </div>
  </div>
</template>

<script>
import Episode from "~/components/episode/Episode.vue";
import CardEpisode from "~/components/episode/CardEpisode.vue";
import Loading from "~/components/Loading.vue";
import axios from "axios";

export default {
  components: { Episode, CardEpisode, Loading },
  data() {
    return {
      episodes: null,
      episodeId: null,
      page: 1,
      pageMax: null,
      error: false,
      loadingStatus: true,
      params: null,
    };
  },
  created: async function () {
    this.createUrl();
    if (this.$route.params.id) {
      this.episodeId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;
      await axios
        .get(
          "https://rickandmortyapi.com/api/episode/?page=" +
            this.page +
            this.params
        )
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
    recherche(event) {
      if (event != 404) {
        if (event.config.url.substring(event.config.url.indexOf("=") + 1) == "")
          this.$router.push({ path: "episodes", query: { page: 1 } });
        else
          this.$router.push({
            path: "episodes",
            query: {
              page: 1,
              name: event.config.url.substring(
                event.config.url.indexOf("=") + 1
              ),
            },
          });
      } else {
        this.error = true;
      }
    },
    createUrl() {
      let urlPara = this.$route.query;
      let paramTempo = "";
      Object.keys(urlPara).map(function (key) {
        if (key != "page" && (key == "name" || key == "status"))
          paramTempo += key + "=" + urlPara[key];
        if (key != Object.keys(urlPara).pop()) paramTempo += "&";
      });
      this.params = paramTempo;
    },
  },
};
</script>

<style  lang='scss' >
</style>