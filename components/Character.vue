<template>
  <div>
    <div>
      <div class="character-img">
        <img :src="character.image" :alt="character.name" style="width: 50%" />
      </div>
    </div>
    name: {{ character.name }} <br />
    <!-- {{ character }} -->
    status: {{ character.status }} <br />
    species: {{ character.species }} <br />
    type: {{ character.type }} <br />
    gender: {{ character.gender }} <br />
    status: {{ character.status }} <br />

    <div>
      <h1>origin</h1>
      <!-- name: {{character.origin.name}} -->
    </div>

    <!-- "origin": { "name": "unknown", "url": "" }, "location": { "name": "Earth (Replacement Dimension)", "url": "https://rickandmortyapi.com/api/location/20" }, "image": "https://rickandmortyapi.com/api/character/avatar/13.jpeg", "episode": [ "https://rickandmortyapi.com/api/episode/31" ], "url": "https://rickandmortyapi.com/api/character/13", "created": "2017-11-04T20:33:30.779Z" } -->

    <!-- {{character.origin.name}} -->
    <!-- {{character.origin.url}} -->

    <div>
      <div class="episode" v-for="item in character.episode" :key="item.id">
        <n-link :to="'/episodes/' + substr(item)">
          <CardEpisode :episodeId="substr(item)" />
        </n-link>
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
    };
  },
  created: async function () {
    let datas = await axios.get(
      "https://rickandmortyapi.com/api/character/" + this.idCharacter
    );
    this.character = datas.data;
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
}
</style>