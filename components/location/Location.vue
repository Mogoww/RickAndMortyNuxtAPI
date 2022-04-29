<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    <Loading v-else-if="loadingStatus" v-show="loadingStatus" />
    <div v-else>
      <div class="grid grid-cols-5">
        <div class="col-span-12 md:col-span-2 flex items-center justify-center">
          <div class="flex content-center justify-center">
            <Like
              :id="'l' + location.id"
              :type="'location'"
              :url="location.url"
              :oneDisplay="true"
            />
          </div>
        </div>
        <div class="col-span-12 md:col-span-3 p-5">
          <div class="bg-white p-8 mx-auto">
            <div>
              <div class="flex items-center mb-5">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Nom de la planète
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ location.name == "unknown" ? "Pas renseigné" : location.name }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Type de planète
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ location.type == "unknown" ? "Pas renseigné" : location.type }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Dimension
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{
                    location.dimension == "unknown" ? "Pas renseigné" : location.dimension
                  }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Carousel des characters présent dans la location -->

      <div v-if="location.residents !== undefined && location.residents.length > 0" class="border-y-4 border-lime-600 py-8">
        <div>
          <h1 class="text-center text-lg font-bold">
            Personnes résident sur cette planète
          </h1>
        </div>
        <Carousel class="px-10" :data="location.residents" :type="'characters'" />
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

<style lang="scss"></style>
