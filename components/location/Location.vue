<template>
  <div>
    <!-- {{location}} -->
    <!-- Carousel des characters présent dans la location -->

    <div v-if="location.residents !== undefined && location.residents.length > 0 ">
      <Carousel class="px-10" :data="location.residents" :type="'characters'" />
    </div>
  </div>
</template>

<script>
import Carousel from "~/components/Carousel.vue";

import axios from "axios";
export default {
  components: { Carousel },
  props: ["locationId"],
  data() {
    return {
      location: [],
    };
  },
  created: async function () {
    let datas = await axios.get(
      "https://rickandmortyapi.com/api/location/" + this.locationId
    );
    this.location = datas.data;
  },
};
</script>

<style  lang='scss' >
</style>