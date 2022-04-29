<template>
  <div>
    <div class="flex items-center justify-center">
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

    <!-- <ul class="grid grid-cols-3 gap-x-5 m-10 max-w-md mx-auto">
      <li class="relative">
        <input
          class="sr-only peer"
          type="radio"
          value="yes"
          name="answer"
          id="character_radio"
          checked
        />
        <label
          class="
            flex
            p-5
            bg-white
            border border-gray-300
            rounded-lg
            cursor-pointer
            focus:outline-none
            hover:bg-gray-50
            peer-checked:ring-green-500
            peer-checked:ring-2
            peer-checked:border-transparent
          "
          for="character_radio"
          >Yes</label
        >

        <div class="absolute hidden w-5 h-5 peer-checked:block top-5 right-3">
          👍
        </div>
      </li>

      <li class="relative">
        <input
          class="sr-only peer"
          type="radio"
          value="no"
          name="answer"
          id="answer_no"
        />
        <label
          class="
            flex
            p-5
            bg-white
            border border-gray-300
            rounded-lg
            cursor-pointer
            focus:outline-none
            hover:bg-gray-50
            peer-checked:ring-red-500
            peer-checked:ring-2
            peer-checked:border-transparent
          "
          for="answer_no"
          >No</label
        >

        <div class="absolute hidden w-5 h-5 peer-checked:block top-5 right-3">
          👎
        </div>
      </li>

      <li class="relative">
        <input
          class="sr-only peer"
          type="radio"
          value="maybe"
          name="answer"
          id="answer_maybe"
        />
        <label
          class="
            flex
            p-5
            bg-white
            border border-gray-300
            rounded-lg
            cursor-pointer
            focus:outline-none
            hover:bg-gray-50
            peer-checked:ring-yellow-500
            peer-checked:ring-2
            peer-checked:border-transparent
          "
          for="answer_maybe"
          >Maybe</label
        >

        <div class="absolute hidden w-5 h-5 peer-checked:block top-5 right-3">
          🤔
        </div>
      </li>
    </ul> -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["parentData",'typeRecherche'],
  data() {
    return {
      name: null,
    };
  },
  methods: {
    recherche: async function () {
      this.name = document.querySelector("#recherche_text").value;
      await axios
        .get("https://rickandmortyapi.com/api/"+this.typeRecherche+"/?name=" + this.name)
        .then((res) => {
          this.$emit("interface", res);
        })
        .catch((error) => {
          this.$emit("interface", 404);
        });
    },
  },
  mounted: function () {
    if (this.$route.query.name)
      document.querySelector("#recherche_text").value = this.$route.query.name;
  },
};
</script>