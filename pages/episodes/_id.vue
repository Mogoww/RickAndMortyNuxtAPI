<template>
  <div>
    <!-- {{episodes}} -->
    <div v-if="episodes">
      <div v-for="item in episodes" :key="item.id">
        <n-link :to="'/episodes/' + substr(item.url)">
          {{ item.name }}
          {{ item.episode }}
          <!-- {{item.url}} -->
          <!-- <CardCharacter :idCharacter="substr(item.url)" /> -->
        </n-link>
      </div>
    </div>

    <!-- <div v-if="episode">
      {{ episode.name }}
      {{ episode.episode }}

      <div style="display: flex; flex-wrap: wrap">
        <div v-for="item in episode.characters" :key="item.id">
          <n-link :to="'/characters/' + substr(item)">
            <CardCharacter :idCharacter="substr(item)" />
          </n-link>
        </div>
      </div>
    </div> -->


    <div v-if="episode" style="margin: 100px">
      {{ episode.name }}
      {{ episode.episode }}

      <!-- <Test :datasss="'episode.episode'" /> -->
      <Carousel :data="episode.characters" :type="'characters'" />

      <!-- <VueSlickCarousel v-bind="settings">
        <div v-for="item in episode.characters" :key="item.id">
          <div class="card-margin">
            <n-link :to="'/characters/' + substr(item)">
              <CardCharacter :idCharacter="substr(item)" />
            </n-link>
          </div>
        </div>
      </VueSlickCarousel> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
// import VueSlickCarousel from "vue-slick-carousel";
import "vue-slick-carousel/dist/vue-slick-carousel-theme.css";

export default {
  // components: { VueSlickCarousel },
  data() {
    return {
      episodes: null,
      episode: null,
      settings: {
        dots: true,
        infinite: false,
        speed: 500,
        slidesToShow: 4,
        slidesToScroll: 4,
        initialSlide: 0,
        arrows: true,
        responsive: [
          {
            breakpoint: 1024,
            settings: {
              slidesToShow: 3,
              slidesToScroll: 3,
              infinite: true,
              dots: true,
            },
          },
          {
            breakpoint: 600,
            settings: {
              slidesToShow: 2,
              slidesToScroll: 2,
              initialSlide: 2,
            },
          },
          {
            breakpoint: 480,
            settings: {
              slidesToShow: 1,
              slidesToScroll: 1,
            },
          },
        ],
      },
    };
  },
  created: async function () {
    let id = this.$route.params.id;
    let datas;
    if (id) {
      datas = await axios.get("https://rickandmortyapi.com/api/episode/" + id);
      this.episode = datas.data;
    } else {
      datas = await axios.get("https://rickandmortyapi.com/api/episode");
      this.episodes = datas.data.results;
    }
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script>

<style  lang='scss' >
.card-margin{
  margin: 10px 30px 10px 30px;
}

button.slick-prev:before,
button.slick-next:before {
  color: black !important;
}
</style>