<template>
  <div>
    <!-- {{location}} -->
    <!-- Carousel des characters présent dans la location -->

    <div
      v-if="location.residents !== undefined && location.residents.length > 0"
    >
      <Carousel class="px-10" :data="location.residents" :type="'characters'" />
    </div>

    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
  </div>
</template>

<script>
import Carousel from "~/components/Carousel.vue";
import NotFoundPage from "~/components/NotFoundPage.vue";


import axios from "axios";
export default {
  components: { Carousel, NotFoundPage },
  props: ["locationId"],
  data() {
    return {
      location: [],
      error: false,
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/location/" + this.locationId)
      .then((res) => {
        this.location = res.data;
        console.log(res);
      })
      .catch((error) => {
        this.error = true;
      });
  },
};
</script>

<style  lang='scss' >
</style>