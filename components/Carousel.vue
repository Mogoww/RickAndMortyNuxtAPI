<template>
  <div>
    <div v-if="data">
      <VueSlickCarousel v-bind="settings">
        <div v-for="item in data" :key="item.id">
          <div class="card-margin">
            <div v-if="type == 'characters'">
              <n-link :to="'/characters/' + substr(item)">
                <CardCharacter :idCharacter="substr(item)" />
              </n-link>
            </div>
          </div>
        </div>
      </VueSlickCarousel>
    </div>
  </div>
</template>
 
<script>
import "vue-slick-carousel/dist/vue-slick-carousel.css";
import "vue-slick-carousel/dist/vue-slick-carousel-theme.css";
import VueSlickCarousel from "vue-slick-carousel";
import CardCharacter from "~/components/character/CardCharacter.vue";
import axios from "axios";

export default {
  props: ["data", "type"],
  name: "Carousel",
  components: { VueSlickCarousel,CardCharacter },
  data() {
    return {
      settings: {
        dots: true,
        infinite: true,
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
              dots: false,
              slidesToShow: 1,
              slidesToScroll: 1,
            },
          },
        ],
      },
    };
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script> 

// Style carousel
<style  lang='scss' >
.card-margin {
  margin: 10px 30px 10px 30px;
  display: flex;
}

button.slick-prev:before,
button.slick-next:before {
  color: black !important;
}
</style>