<template>
  <div>
    <!-- Aucun résultat -->
    <div v-if="error">
      <NotFoundPage />
    </div>

    <div v-else>
      <div>
        <div class="character-img">
          <img :src="character.image" :alt="character.name" />
        </div>
      </div>

      <div>
        <div class="bg-green-200 py-32 px-10">
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
                  {{ character.name }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="flex relative">
                  <div
                    class="
                      relative
                      inline-block
                      w-20
                      mr-6
                      text-right
                      font-bold
                      text-gray-600
                    "
                  >
                    Status
                  </div>
                  <div v-if="character.status == 'Alive'">
                    <div
                      class="
                        absolute
                        right-4
                        shadow-sm
                        bg-green-500
                        w-2
                        h-2
                        rounded-full
                      "
                    ></div>
                  </div>
                  <div v-else-if="character.status == 'Dead'">
                    <div
                      class="
                        absolute
                        right-4
                        shadow-sm
                        bg-red-600
                        w-2
                        h-2
                        rounded-full
                      "
                    ></div>
                  </div>
                  <div v-else>
                    <div
                      class="
                        absolute
                        right-4
                        shadow-sm
                        bg-gray-600
                        w-2
                        h-2
                        rounded-full
                      "
                    ></div>
                  </div>
                </div>
                <div
                  class="
                    flex-1 flex
                    py-2
                    border-b-2 border-gray-400
                    focus:border-green-400
                    text-gray-600
                    placeholder-gray-400
                    outline-none
                  "
                >
                  <span v-if="character.status == 'unknown'"> Manquant </span>
                  <span v-else>
                    {{ character.status }}
                  </span>
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
                  species
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
                  {{ character.species }}
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
                  gender
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
                  {{ character.gender }}
                </div>
              </div>

              <div v-if="character.type" class="flex items-center mb-10">
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
                  {{ character.type }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div
        :class="
          character.origin &&
          character.origin.url &&
          character.location &&
          character.location.url
            ? 'md:grid-cols-2'
            : ''
        "
        class="grid grid-cols-1 place-items-center"
      >
        <div v-if="character.origin && character.origin.url">
          <h1>origin</h1>
          <div>
            <n-link :to="'/locations/' + substr(character.origin.url)">
              <CardLocation :locationId="substr(character.origin.url)" />
            </n-link>
          </div>
        </div>

        <div v-if="character.location && character.location.url">
          <h1>location</h1>
          <div>
            <n-link :to="'/locations/' + substr(character.location.url)">
              <CardLocation :locationId="substr(character.location.url)" />
            </n-link>
          </div>
        </div>
      </div>

      <div class="">
        <div v-for="(items, index) in episodes.res" v-bind:key="index">
          <div
            v-if="items != null && items.numEp.length > 0"
            class="border-t-4 border-b-4 pt-2 border-double"
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
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/character/" + this.idCharacter)
      .then((res) => {
        this.character = res.data;
        this.episodes = this.triEpisode(this.character.episode);
      })
      .catch((error) => {
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
        count: [
          S01.length,
          S02.length,
          S03.length,
          S04.length,
          S05.length,
        ].filter(Boolean).length,
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
    updateNameSaison: function(data){
      return "Saison "+ data.split('S').pop()
    }
  },
};
</script>

<style lang='scss' scoped>
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

.episode:hover {
  background: #8cc961;
}
.image-saison {
  width: 200px;
}
</style>