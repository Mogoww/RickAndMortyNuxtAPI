<template>
  <div class="location text-white font-bold py-2 px-4 rounded">
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
.location {
  background: linear-gradient(to bottom right, #e5928b, #f9c593);
  margin: 10px;
  text-align: center;
}
</style>