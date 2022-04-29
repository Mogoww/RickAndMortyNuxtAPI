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
          <div class="my-2.5">
            <img
              class="img-episode"
              :src="require('~/assets/images/' + substrSaison(episode.episode) + '.jpg')"
            />
            <div class="flex content-center justify-center">
              <Like
                :id="'e' + episode.id"
                :type="'episode'"
                :url="episode.url"
                :oneDisplay="true"
              />
            </div>
          </div>
        </div>
        <div class="col-span-12 md:col-span-3 p-5">
          <div class="bg-white p-8 mx-auto">
            <div>
              <div class="flex items-center mb-5">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Nom
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ episode.name }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Date de sortie
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{
                    new Date(episode.air_date).toLocaleString("fr-FR", {
                      weekday: "long",
                      day: "2-digit",
                      year: "numeric",
                      month: "long",
                    })
                  }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Épisode
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ episode.episode }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="border-y-4 border-lime-600 py-8">
        <h1 class="text-center text-lg font-bold">
          Personnages présents dans cet épisode
        </h1>
        <!-- Carousel des characters présent dans l'épisode -->
        <Carousel :data="episode.characters" :type="'characters'" class="px-8" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["episodeId"],
  data() {
    return {
      episode: [],
      error: false,
      loadingStatus: true,
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/episode/" + this.episodeId)
      .then((res) => {
        this.episode = res.data;
        this.loadingStatus = false;
      })
      .catch((error) => {
        this.error = true;
        this.loadingStatus = false;
      });
  },
  methods: {
    substrSaison: function (data) {
      if (data != null) return data.substring(0, data.indexOf("E"));
    },
  },
};
</script>

<style lang="scss" scoped>
@import "~/assets/scss/variables";

.img-episode {
  height: 400px;
}
</style>
