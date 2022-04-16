<template>
  <div>
    <img :src="character.image" :alt="character.name" style="width: 50%" />
    {{ character.name }}
    {{ character }}

    <!-- {{character.origin.name}} -->

    <!-- {{character.origin.url}} -->
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
      // this.substr = this.str.substr(1, 4);
    },
  },
};
</script>

<style lang='scss' scoped>
@import "~/assets/scss/variables";
</style>