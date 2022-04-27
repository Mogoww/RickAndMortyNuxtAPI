<template>
  <div>
    <!-- Loading  -->
    <loading v-show="loadingStatus" />

    <!-- List épisodes -->
    <div v-if="locations">
      <div v-for="item in locations" :key="item.id" class="relative">
        <n-link :to="'/locations/' + substr(item.url)">
          <CardLocation :locationId="substr(item.url)" />
        </n-link>
         <Like
          :id="'l' + substr(item.url)"
          :type="'location'"
          :url="item.url"
          class="absolute like"
        />
      </div>
      <Pagination :pageNum="page" :pageMax="pageMax" />
    </div>

    <!-- One location -->
    <div v-if="locationId">
      <Location :locationId="locationId" />
    </div>

    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
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
    };
  },
  created: async function () {
    if (this.$route.params.id) {
      this.locationId = this.$route.params.id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) this.page = this.$route.query.page;

      await axios
        .get("https://rickandmortyapi.com/api/location/?page=" + this.page)
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
  },
};
</script>

<style  lang='scss' >
.like {
  right: 18px;
  bottom: 15px;
}
</style>