<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>
    <Loading v-else-if="loadingStatus" v-show="loadingStatus" />
    <div v-else>
      <div v-if="episode.episode != null">
        <div class="flex items-center justify-center">
          <img
            class="img-episode"
            :src="
              require('~/assets/images/' +
                substrSaison(episode.episode) +
                '.jpg')
            "
          />
        </div>
      </div>

      <div class="bg-green-200 py-32 px-10">
        <div class="flex content-center justify-center">
          <Like
            :id="'e' + episode.id"
            :type="'episode'"
            :url="episode.url"
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
                {{ episode.name }}
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
                Date
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
                {{ episode.air_date }}
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
                episode
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
                {{ episode.episode }}
              </div>
            </div>
          </div>
        </div>
      </div>
      <div>
        <h1>Personnages présent dans cette épisode</h1>
        <!-- Carousel des characters présent dans l'épisode -->
        <Carousel
          :data="episode.characters"
          :type="'characters'"
          class="px-8"
        />
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