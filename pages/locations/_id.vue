<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    <div v-else>
      <!-- List épisodes -->
      <div v-if="locations">
        <!-- Loading  -->
        <Loading v-show="loadingStatus" />

        <!-- Recherche par nom -->
        <Recherche :typeRecherche="'location'" @interface="recherche" />

        <div v-for="item in locations" :key="item.id" class="relative">
          <n-link :to="'/locations/' + substr(item.url)">
            <CardLocation :locationId="substr(item.url)" />
          </n-link>
          <Like
            :id="'l' + substr(item.url)"
            :type="'location'"
            :url="item.url"
          />
        </div>
        <Pagination :pageNum="page" :pageMax="pageMax" />
      </div>

      <!-- One location -->
      <div v-if="locationId">
        <Location :locationId="locationId" />
      </div>
    </div>
  </div>
</template>

<script>
import Location from "~/components/location/Location.vue";
import CardLocation from "~/components/location/CardLocation.vue";
import axios from "axios";

export default {
  components: { Location, CardLocation },
  data() {
    return {
      locations: null,
      locationId: null,
      page: 1,
      pageMax: null,
      error: false,
      loadingStatus: true,
      params: null,
    };
  },
  created: async function () {
    if (this.$route.params.id) {
      this.locationId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;
      this.createUrl();

      await axios
        .get(
          "https://rickandmortyapi.com/api/location/?page=" +
            this.page +
            this.params
        )
        .then((res) => {
          this.locations = res.data.results;
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
          this.$router.push({ path: "locations", query: { page: 1 } });
        else
          this.$router.push({
            path: "locations",
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
.like {
  // right: 18px;
  // top: 50%;
  // transform: translateY(-50%);
}
</style>