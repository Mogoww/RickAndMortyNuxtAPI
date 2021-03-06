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
          <div>
            <img :src="character.image" :alt="character.name" />
            <div class="flex content-center justify-center">
              <Like
                :id="'c' + character.id"
                :type="'character'"
                :url="character.url"
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
                  {{ character.name == "unknown" ? "Pas renseigné" : character.name }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="flex relative">
                  <div
                    class="relative inline-block w-20 mr-6 text-left font-bold text-gray-600"
                  >
                    Status
                  </div>
                  <div v-if="character.status == 'Alive'">
                    <div
                      class="absolute right-4 shadow-sm bg-green-500 w-2 h-2 rounded-full"
                    ></div>
                  </div>
                  <div v-else-if="character.status == 'Dead'">
                    <div
                      class="absolute right-4 shadow-sm bg-red-600 w-2 h-2 rounded-full"
                    ></div>
                  </div>
                  <div v-else>
                    <div
                      class="absolute right-4 shadow-sm bg-blue-500 w-2 h-2 rounded-full"
                    ></div>
                  </div>
                </div>
                <div
                  class="flex-1 flex py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ character.status == "unknown" ? "Pas renseigné" : character.status }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Espèces
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{
                    character.species == "unknown" ? "Pas renseigné" : character.species
                  }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Genre
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ character.gender == "unknown" ? "Pas renseigné" : character.gender }}
                </div>
              </div>

              <div v-if="character.type" class="flex items-center mb-10">
                <div class="inline-block w-20 mr-6 text-left font-bold text-gray-600">
                  Type
                </div>

                <div
                  class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none"
                >
                  {{ character.type == "unknown" ? "Pas renseigné" : character.type }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        :class="[
          character.origin &&
          character.origin.url &&
          character.location &&
          character.location.url
            ? 'md:grid-cols-2'
            : '',
          (character.origin && character.origin.url) ||
          (character.location && character.location.url)
            ? 'border-t-4 border-lime-600'
            : '',
        ]"
        class="grid grid-cols-1"
      >
        <div v-if="character.origin && character.origin.url">
          <h1 class="text-center text-lg font-bold">Planète d'origine</h1>
          <div>
            <n-link :to="'/locations/' + substr(character.origin.url)">
              <CardLocation :locationId="substr(character.origin.url)" />
            </n-link>
          </div>
        </div>

        <div v-if="character.location && character.location.url">
          <h1 class="text-center text-lg font-bold">Dernière localisation</h1>
          <div>
            <n-link :to="'/locations/' + substr(character.location.url)">
              <CardLocation :locationId="substr(character.location.url)" />
            </n-link>
          </div>
        </div>
      </div>

      <div>
        <div v-for="(items, index) in episodes.res" v-bind:key="index">
          <div
            v-if="items != null && items.numEp.length > 0"
            class="border-t-4 border-lime-600 pt-2.5"
          >
            <div class="flex justify-center">
              <div class="image-saison">
                <img
                  :src="require('~/assets/images/' + items.saison + '.jpg')"
                  :alt="items.saison"
                />
                <div class="flex justify-center text-xl italic">
                  {{ updateNameSaison(items.saison) }}
                </div>
              </div>
            </div>
            <div class="flex flex-wrap justify-center">
              <div v-for="item in items.numEp" :key="item.id">
                <n-link :to="'/episodes/' + item.num">
                  <CardEpisode :episodeId="item.num" />
                </n-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardEpisode from "~/components/episode/CardEpisode.vue";
import CardLocation from "~/components/location/CardLocation.vue";

import axios from "axios";
export default {
  components: {
    CardEpisode,
    CardLocation,
  },
  props: ["idCharacter"],
  data() {
    return {
      character: [],
      error: false,
      episodes: [],
      loadingStatus: true,
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/character/" + this.idCharacter)
      .then((res) => {
        this.character = res.data;
        this.episodes = this.triEpisode(this.character.episode);
        this.loadingStatus = false;
      })
      .catch((error) => {
        this.loadingStatus = false;
        this.error = true;
      });
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
    triEpisode: function (data) {
      let tab = [];
      let S01 = [];
      let S02 = [];
      let S03 = [];
      let S04 = [];
      let S05 = [];
      data.map(function (value, key) {
        let number = parseInt(value.substring(value.lastIndexOf("/") + 1));

        if (number >= 1 && number <= 11) {
          S01.push({ num: number });
        } else if (number >= 12 && number <= 21) {
          S02.push({ num: number });
        } else if (number >= 22 && number <= 31) {
          S03.push({ num: number });
        } else if (number >= 32 && number <= 41) {
          S04.push({ num: number });
        } else if (number >= 42 && number <= 51) {
          S05.push({ num: number });
        }
      });

      tab.push({
        count: [S01.length, S02.length, S03.length, S04.length, S05.length].filter(
          Boolean
        ).length,
        res: [
          S01.length != 0 ? { saison: "S01", numEp: S01 } : null,
          S02.length != 0 ? { saison: "S02", numEp: S02 } : null,
          S03.length != 0 ? { saison: "S03", numEp: S03 } : null,
          S04.length != 0 ? { saison: "S04", numEp: S04 } : null,
          S05.length != 0 ? { saison: "S05", numEp: S05 } : null,
        ],
      });

      return tab[0];
    },
    updateNameSaison: function (data) {
      return "Saison " + data.split("S").pop();
    },
  },
};
</script>

<style lang="scss" scoped>
@import "~/assets/scss/variables";
.character-img {
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #cbcbcb;
  img {
    width: 400px;
  }
}

.image-saison {
  width: 200px;
}
</style>
