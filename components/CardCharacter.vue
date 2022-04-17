<template>
  <div class="card">
    <div>
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
    console.log(datas.data);
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
  width: 200px;
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
</style>