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
                <div class=" inline-block w-20 mr-6 text-right font-bold text-gray-600">
                  Nom
                </div>

                <div class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none" >
                  {{ character.name }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class="flex relative">
                  <div class="relative inline-block w-20 mr-6 text-right font-bold text-gray-600">
                    Status                  
                  </div>
                  <div v-if="character.status == 'Alive'">
                    <div class="absolute right-4 shadow-sm bg-green-500 w-2 h-2 rounded-full"></div>
                  </div>
                  <div v-else-if="character.status == 'Dead'">
                    <div class="absolute right-4 shadow-sm bg-red-600 w-2 h-2 rounded-full"></div>
                  </div>
                  <div v-else>
                    <div class="absolute right-4 shadow-sm bg-gray-600 w-2 h-2 rounded-full"></div>
                  </div>

                </div>
                <div class="flex-1 flex py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none" >
                  <span v-if="character.status == 'unknown' ">
                    Manquant                  
                  </span>
                  <span v-else>
                    {{ character.status }} 
                  </span>
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class=" inline-block w-20 mr-6 text-right font-bold text-gray-600">
                  species
                </div>

                <div class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none" >
                  {{ character.species }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class=" inline-block w-20 mr-6 text-right font-bold text-gray-600">
                  gender
                </div>

                <div class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none" >
                  {{ character.gender }}
                </div>
              </div>

              <div class="flex items-center mb-10">
                <div class=" inline-block w-20 mr-6 text-right font-bold text-gray-600">
                  type
                </div>

                <div class="flex-1 py-2 border-b-2 border-gray-400 focus:border-green-400 text-gray-600 placeholder-gray-400 outline-none" >
                  <!-- {{ character }} -->
                </div>
              </div>

             
            </div>
          </div>
        </div>
      </div>

      <!-- <div>name: {{ character.name }}</div>
      <div>status: {{ character.status }}</div>
      <div>species: {{ character.species }}</div>
      <div>gender: {{ character.gender }}</div>
      <div>type: {{ character.type }}</div>
      <div>status: {{ character.status }}</div>

      <div>
        <h1>origin</h1>
        <div v-if="character.origin">
          {{ character.origin }}
          {{ character.origin["name"] }}
        </div>
        <div v-if="character.origin">
          {{ character.origin["url"] }}
        </div>
     {{character.origin["url"]}} -->
      <!-- </div>

      <div>
        <h1>location</h1>
        <div v-if="character.location">
          {{ character.location }}
          {{ character.location["name"] }}
        </div>
        <div v-if="character.origin">
          {{ character.location["url"] }}
        </div>
        {{character.origin["url"]}} -->
      <!--  </div> -->
      <!-- "origin": { "name": "unknown", "url": "" }, "location": { "name": "Earth (Replacement Dimension)", "url": "https://rickandmortyapi.com/api/location/20" }-->

      <!-- {{character.origin.name}} -->
      <!-- {{character.origin.url}} -->

      <div class="flex flex-wrap">
        <div v-for="item in character.episode" :key="item.id">
          <div class="episode text-white font-bold py-2 px-4 rounded">
            <n-link :to="'/episodes/' + substr(item)">
              <CardEpisode :episodeId="substr(item)" />
            </n-link>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
import axios from "axios";
export default {
  props: ["idCharacter"],
  data() {
    return {
      character: [],
      error: false,
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/character/" + this.idCharacter)
      .then((res) => {
        this.character = res.data;
      })
      .catch((error) => {
        this.error = true;
      });
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
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
.episode {
  background: linear-gradient(to bottom right, #9bca3d, #2e8841);
  margin: 10px;
}
.episode:hover {
  background: #8cc961;
}
</style>