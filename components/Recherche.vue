<template>
  <div>
    <div class="flex items-center justify-center py-6">
      <div class="flex border-2 rounded">
        <input
          type="text"
          class="px-4 py-2 w-80"
          placeholder="Taper le nom ..."
          id="recherche_text"
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

    <ul
      class="grid grid-cols-3 gap-x-5 m-10 max-w-md mx-auto"
      v-if="typeRecherche == 'characters'"
    >
      <li class="relative">
        <input
          class="sr-only peer"
          type="radio"
          value="alive"
          name="answer"
          id="answer_alive"
          @click="status"
        />
        <label
          class="flex p-5 bg-white border border-gray-300 rounded-lg cursor-pointer focus:outline-none hover:bg-gray-50 peer-checked:ring-green-500 peer-checked:ring-2 peer-checked:border-transparent"
          for="answer_alive"
          >En vie</label
        >

        <div class="absolute hidden w-5 h-5 peer-checked:block top-5 right-3">👍</div>
      </li>

      <li class="relative">
        <input
          class="sr-only peer"
          type="radio"
          value="dead"
          name="answer"
          id="answer_dead"
          @click="status"
        />
        <label
          class="flex p-5 bg-white border border-gray-300 rounded-lg cursor-pointer focus:outline-none hover:bg-gray-50 peer-checked:ring-red-500 peer-checked:ring-2 peer-checked:border-transparent"
          for="answer_dead"
          >Mort</label
        >

        <div class="absolute hidden w-5 h-5 peer-checked:block top-5 right-3">👎</div>
      </li>

      <li class="relative">
        <input
          class="sr-only peer"
          type="radio"
          value="unknown"
          name="answer"
          id="answer_unknown"
          @click="status"
        />
        <label
          class="flex p-5 bg-white border border-gray-300 rounded-lg cursor-pointer focus:outline-none hover:bg-gray-50 peer-checked:ring-red-500 peer-checked:ring-2 peer-checked:border-transparent"
          for="answer_unknown"
          >Non renseigné</label
        >

        <div class="absolute hidden w-5 h-5 peer-checked:block top-5 right-3">👎</div>
      </li>
    </ul>
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
      this.name = document.querySelector("#recherche_text").value;
      let data = {
        page: 1,
      };
      if (this.name && this.$route.query.name != this.name) data.name = this.name;
      if (this.$route.query.status) data.status = this.$route.query.status;

      this.$router.push({
        path: this.typeRecherche,
        replace: true,
        query: data,
      });
    },
    status: function (e) {
      let data = {
        page: 1,
      };
      if (this.$route.query.name) data.name = this.$route.query.name;
      if (!this.$route.query.status || this.$route.query.status != e.srcElement.value)
        data.status = e.srcElement.value;
      this.$router.push({
        path: this.typeRecherche,
        replace: true,
        query: data,
      });
    },
  },
  mounted: function () {
    if (this.$route.query.name)
      document.querySelector("#recherche_text").value = this.$route.query.name;
    if(this.$route.query.status){
      console.log(this.$route.query.status);
      document.querySelector("#answer_"+this.$route.query.status).checked = true
    }
  },
};
</script>
