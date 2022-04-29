<template>
  <div style="">
    <div
      v-if="
        characters.length > 0 || episodes.length > 0 || locations.length > 0
      "
    >
      <div
        class="
          text-6xl text-center
          font-normal
          leading-normal
          mt-0
          mb-2
          text-pink-800
        "
      >
        Vos likes
      </div>

      <!-- list characters -->
      <div v-if="characters.length > 0">
        <div>Vos personnages préféré</div>
        <div
          class="container-character flex flex-wrap items-center justify-center"
        >
          <div
            class="card relative"
            v-for="(item, index) in characters"
            v-bind:key="index"
          >
            <div v-if="item != null">
              <n-link :to="'/characters/' + substr(item)">
                <CardCharacter :idCharacter="substr(item)" />
              </n-link>
              <Like :id="'c' + substr(item)" :type="'character'" :url="item" />
            </div>
          </div>
        </div>
      </div>

      <!-- List épisodes -->
      <div v-if="episodes.length > 0">
        <div>Vos episodes préféré</div>
        <div
          v-for="(item, index) in episodes"
          v-bind:key="index"
          class="relative"
        >
          <div v-if="item != null">
            <n-link :to="'/episodes/' + substr(item)">
              <CardEpisode :episodeId="substr(item)" />
            </n-link>
            <Like :id="'e' + substr(item)" :type="'episode'" :url="item" />
          </div>
        </div>
      </div>

      <!-- List épisodes -->
      <div v-if="locations.length > 0">
        <div>Vos locations préféré</div>
        <div
          v-for="(item, index) in locations"
          v-bind:key="index"
          class="relative"
        >
          <div v-if="item != null">
            <n-link :to="'/locations/' + substr(item)">
              <CardLocation :locationId="substr(item)" />
            </n-link>
            <Like :id="'l' + substr(item)" :type="'location'" :url="item" />
          </div>
        </div>
      </div>
    </div>
    <div v-else id="presentation-like" class="flex justify-center items-center">
      <div class="text-4xl text-center">
        Vous pouvez liker des episodes, personne ou localisation. ils seront
        visible ici
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container-character {
  .card {
    padding: 10px;
  }
}
</style>

<script>
import axios from "axios";
import CardCharacter from "~/components/character/CardCharacter.vue";
import CardEpisode from "~/components/episode/CardEpisode.vue";
import CardLocation from "~/components/location/CardLocation.vue";

export default {
  name: "IndexPage",
  components: {
    CardCharacter,
    CardEpisode,
    CardLocation,
  },
  data() {
    return {
      characters: [],
      episodes: [],
      locations: [],
    };
  },
  mounted() {
    if (localStorage.getItem("character")) {
      let tabTempo = [];
      tabTempo = JSON.parse(localStorage.getItem("character"));
      this.characters = tabTempo;
    }
    if (localStorage.getItem("episode")) {
      let tabTempo = [];
      tabTempo = JSON.parse(localStorage.getItem("episode"));
      this.episodes = tabTempo;
      console.log(localStorage.getItem("episode"));
    }
    if (localStorage.getItem("location")) {
      let tabTempo = [];
      tabTempo = JSON.parse(localStorage.getItem("location"));
      this.locations = tabTempo;
      console.log(localStorage.getItem("location"));
    }

    window.addEventListener("resize", this.resizeBody);
    this.resizeBody();
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
    resizeBody() {
      let header = document.querySelector("#header");
      let html = document.documentElement;
      document.querySelector("#presentation-like").style.minHeight =
        html.clientHeight - header.offsetHeight + "px";
    },
  },
};
</script>
