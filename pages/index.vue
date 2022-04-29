<template>
  <div>
    <div v-if="characters.length > 0 || episodes.length > 0 || locations.length > 0">
      <div
        class="text-6xl text-center font-normal leading-normal mt-0 mb-2 text-pink-800"
      >
        VOS LIKES
      </div>

      <!-- list characters -->
      <div v-if="characters.length > 0">
        <div class="flex items-center justify-center mt-4">
          <div class="text-3xl title">
            Vos personnages préférés
            <Toggle @update:parent="toggleCharacter = !$event" class="toggle" />
          </div>
        </div>

        <div
          class="container-character flex flex-wrap items-center justify-center"
          :class="{ hidden: toggleCharacter }"
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
        <div class="flex items-center justify-center mt-4">
          <div class="text-3xl title">
            Vos épisodes préférés
            <Toggle @update:parent="toggleEpisode = !$event" class="toggle" />
          </div>
        </div>

        <div
          v-for="(item, index) in episodes"
          v-bind:key="index"
          class="relative"
          :class="{ hidden: toggleEpisode }"
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
        <div class="flex items-center justify-center mt-4">
          <div class="text-3xl title">
            Vos planètes préférées
            <Toggle @update:parent="toggleLocation = !$event" class="toggle" />
          </div>
        </div>

        <div
          v-for="(item, index) in locations"
          v-bind:key="index"
          class="relative"
          :class="{ hidden: toggleLocation }"
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
        Bienvenue sur le site de la série<br />
        <strong>Rick et Morty</strong><br /><br />

        Vous pouvez liker des épisodes, personnages ou localisations et les retrouver sur
        cette page !!!
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
#presentation-like {
  div {
    margin: 100px;
  }
}
.title {
  position: relative;
}
.toggle {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: -80px;
}
@media only screen and (max-width: 600px) {
  .title {
    position: static;
  }
  .toggle {
    justify-content: center;
    position: static;
    transform: translateY(0);
    top: 0;
    right: 0;
  }
}
</style>

<script>
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
      toggleCharacter: false,
      toggleEpisode: false,
      toggleLocation: false,
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
    }
    if (localStorage.getItem("location")) {
      let tabTempo = [];
      tabTempo = JSON.parse(localStorage.getItem("location"));
      this.locations = tabTempo;
    }
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script>
