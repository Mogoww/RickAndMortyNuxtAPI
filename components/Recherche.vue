<template>
  <div>
    <div class="flex items-center justify-center py-6">
      <div class="flex border-2 rounded">
        <input
          type="text"
          class="px-4 py-2 w-80"
          placeholder="Taper le nom ..."
          id="name"
        />
        <button
          v-on:click="recherche"
          class="flex items-center justify-center px-4 border-l bg-white"
          id="recherche_btn"
        >
          <svg
            class="w-6 h-6 text-gray-600"
            fill="currentColor"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 24 24"
          >
            <path
              d="M16.32 14.9l5.39 5.4a1 1 0 0 1-1.42 1.4l-5.38-5.38a8 8 0 1 1 1.41-1.41zM10 16a6 6 0 1 0 0-12 6 6 0 0 0 0 12z"
            />
          </svg>
        </button>
      </div>
    </div>
    <div v-if="typeRecherche == 'characters'" class="flex justify-center flex-wrap">
      <div class="p-5">
        <div>
          <div class="relative inline-flex self-center">
            <ion-icon
              name="chevron-down"
              class="absolute top-0 right-0 text-3xl rounded cursor-pointer"
            ></ion-icon>
            <select
              id="status"
              @change="status($event, 'status')"
              class="font-bold rounded border-2 border-purple-700 text-gray-600 pl-5 pr-10 bg-white hover:border-gray-400 focus:outline-none appearance-none cursor-pointer"
            >
              <option value="" selected>Status :</option>
              <option value="alive">🟢 En vie</option>
              <option value="dead">🔴 Mort</option>
              <option value="unknown">🔵 Non renseigné</option>
            </select>
          </div>
        </div>
      </div>
      <div class="p-5">
        <div>
          <div class="relative inline-flex self-center">
            <ion-icon
              name="chevron-down"
              class="absolute top-0 right-0 text-3xl rounded cursor-pointer"
            ></ion-icon>
            <select
              id="gender"
              @change="status($event, 'gender')"
              class="font-bold rounded border-2 border-purple-700 text-gray-600 pl-5 pr-10 bg-white hover:border-gray-400 focus:outline-none appearance-none cursor-pointer"
            >
              <option value="" selected>Genre :</option>
              <option value="female">Femme</option>
              <option value="male">Homme</option>
              <option value="genderless">Sans sexe</option>
              <option value="unknown">Non renseigné</option>
            </select>
          </div>
        </div>
      </div>
    </div>

    <div v-if="typeRecherche == 'episodes'" class="flex justify-center flex-wrap">
      <div class="p-5">
        <div>
          <div class="relative inline-flex self-center">
            <ion-icon
              name="chevron-down"
              class="absolute top-0 right-0 text-3xl rounded cursor-pointer"
            ></ion-icon>
            <select
              id="episode"
              @change="status($event, 'episode')"
              class="font-bold rounded border-2 border-purple-700 text-gray-600 pl-5 pr-10 bg-white hover:border-gray-400 focus:outline-none appearance-none cursor-pointer"
            >
              <option value="" selected>Saison :</option>
              <option value="S01">Saison 01</option>
              <option value="S02">Saison 02</option>
              <option value="S03">Saison 03</option>
              <option value="S04">Saison 04</option>
              <option value="S05">Saison 05</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["parentData", "typeRecherche"],
  data() {
    return {
      name: null,
    };
  },
  methods: {
    recherche: function () {
      this.name = document.querySelector("#name").value;
      let data = {
        page: 1,
      };
      if (this.name && this.$route.query.name != this.name) data.name = this.name;

      let url = this.$route.query;
      Object.keys(url).map(function (key) {
        if (key != "page" && "name" != key) data[key] = url[key];
      });

      this.$router.push({
        path: this.typeRecherche,
        replace: true,
        query: data,
      });
    },
    status: function (event, type) {
      let data = {
        page: 1,
      };
      let url = this.$route.query;
      Object.keys(url).map(function (key) {
        if (key != "page" && type != key) data[key] = url[key];
      });

      if (this.$route.query.name) data.name = this.$route.query.name;
      if (event.target.value != "") data[type] = event.target.value;
      this.$router.push({
        path: this.typeRecherche,
        replace: true,
        query: data,
      });
    },
  },
  mounted: function () {
    let data = this.$route.query;
    Object.keys(data).map(function (key) {
      console.log(key);
      let el = document.querySelector("#" + key);
      if (el != null) el.value = data[key];
    });
  },
};
</script>
