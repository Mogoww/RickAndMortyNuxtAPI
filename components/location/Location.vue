<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    <Loading v-else-if="loadingStatus" v-show="loadingStatus" />
    <div v-else>
      <div class="bg-green-200 py-32 px-10">
        <div class="flex content-center justify-center">
          <Like
            :id="'l' + location.id"
            :type="'location'"
            :url="location.url"
            :oneDisplay="true"
          />
        </div>
        <div class="bg-white p-10 md:w-3/4 lg:w-1/2 mx-auto">
          <div>
            <div class="flex items-center mb-5">
              <div
                class="
                  inline-block
                  w-20
                  mr-6
                  text-right
                  font-bold
                  text-gray-600
                "
              >
                Nom
              </div>

              <div
                class="
                  flex-1
                  py-2
                  border-b-2 border-gray-400
                  focus:border-green-400
                  text-gray-600
                  placeholder-gray-400
                  outline-none
                "
              >
                {{ location.name }}
              </div>
            </div>

            <div class="flex items-center mb-10">
              <div
                class="
                  inline-block
                  w-20
                  mr-6
                  text-right
                  font-bold
                  text-gray-600
                "
              >
                type
              </div>

              <div
                class="
                  flex-1
                  py-2
                  border-b-2 border-gray-400
                  focus:border-green-400
                  text-gray-600
                  placeholder-gray-400
                  outline-none
                "
              >
                {{ location.type }}
              </div>
            </div>

            <div class="flex items-center mb-10">
              <div
                class="
                  inline-block
                  w-20
                  mr-6
                  text-right
                  font-bold
                  text-gray-600
                "
              >
                dimension
              </div>

              <div
                class="
                  flex-1
                  py-2
                  border-b-2 border-gray-400
                  focus:border-green-400
                  text-gray-600
                  placeholder-gray-400
                  outline-none
                "
              >
                {{ location.dimension }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Carousel des characters présent dans la location -->

      <div
        v-if="location.residents !== undefined && location.residents.length > 0"
      >
      <div><h1>Personnes résident sur cette planete</h1></div>
        <Carousel
          class="px-10"
          :data="location.residents"
          :type="'characters'"
        />
      </div>
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
      loadingStatus: true,
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/location/" + this.locationId)
      .then((res) => {
        this.location = res.data;
        this.loadingStatus = false;
      })
      .catch((error) => {
        this.error = true;
        this.loadingStatus = false;
      });
  },
};
</script>

<style  lang='scss' >
</style>