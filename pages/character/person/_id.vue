<template>
  <div>{{perso.name}}</div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      perso: [],
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas = await axios.get(
      "https://rickandmortyapi.com/api/character/" + id
    );
    this.perso = datas.data;
  },
};
</script>