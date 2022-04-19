<template>
  <div>
    <!-- Loading  -->
    <loading v-show="loadingStatus" />

    <!-- List épisodes -->
    <div v-if="locations">
      <div v-for="item in locations" :key="item.id">
        <n-link :to="'/locations/' + item.id">
          <CardLocation :location="item" />
        </n-link>
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
    let id = this.$route.params.id;

    if (id) {
      this.locationId = id;
      this.loadingStatus = false;
    } else {
      if (this.$route.query.page) {
        this.page = this.$route.query.page;
      }
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
</style>