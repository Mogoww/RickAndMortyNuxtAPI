<template>
  <div>
    <VueSlickCarousel v-bind="settings">
      <div v-for="item in data" :key="item.id">
        <div class="card-margin">
          <div v-if="type == 'characters'">
            <n-link :to="'/characters/' + substr(item)">
              <CardCharacter :idCharacter="substr(item)" />
            </n-link>
          </div>
          <!-- <div v-else-if="type == 'chadddracters'"></div> -->
        </div>
      </div>
    </VueSlickCarousel>
  </div>
</template>
 
<script>
import "vue-slick-carousel/dist/vue-slick-carousel.css";
import "vue-slick-carousel/dist/vue-slick-carousel-theme.css";
import VueSlickCarousel from "vue-slick-carousel";

export default {
  props: ["data", "type"],
  name: "Carousel",
  components: { VueSlickCarousel },
  data() {
    return {
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
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script> 