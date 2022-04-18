<template>
  <div class="card">
    <div class="image-div">
      <img
        class="image-character"
        :src="character.image"
        :alt="character.name"
      />
    </div>
    <div class="container">
      <h4>
        <b>{{ character.name }}</b>
      </h4>
      <p></p>
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
};
</script>

<style lang='scss' scoped>
@import "~/assets/scss/variables";
.card {
  border-radius: 10px;
  text-align: center;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  width: 400px;
}
.image-div {
  display: flex;
  align-items: center;
  justify-content: center;
}
.image-character {
  border-radius: 10px 10px 0 0;
}

/* On mouse-over, add a deeper shadow */
.card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}

/* Add some padding inside the card container */
.container {
  padding: 2px 16px;
}
@media only screen and (max-width: 600px) {
  .card {
    width: 150px;
  }
}
</style>