<template>
  <div>
    {{ location.name }}
  </div>
</template>

<script>
import axios from "axios";

export default {
  props: ["locationId"],
  data() {
    return {
      location: [],
      error: false,
    };
  },
  created: async function () {
    await axios
      .get("https://rickandmortyapi.com/api/location/" + this.locationId)
      .then((res) => {
        this.location = res.data;
      })
      .catch((error) => {
        this.error = true;
      });
  },
};
</script>

<style  lang='scss' >
</style>