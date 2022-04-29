<template>
  <div
    :class="[
      type == 'character' ? 'like-chara' : 'like',
      oneDisplay == true ? '' : 'absolute',
    ]"
  >
    <div
      class="cursor-pointer flex items-center"
      :class="oneDisplay == true ? 'text-6xl' : 'text-3xl'"
    >
      <ion-icon
        :id="'no-like' + id"
        class="text-red-700"
        name="heart-outline"
      ></ion-icon>
      <ion-icon
        :id="'yes-like' + id"
        class="hidden text-red-700"
        name="heart-sharp"
      ></ion-icon>
    </div>
  </div>
</template>

<script>
export default {
  name: "Like",
  props: ["id", "url", "type", "oneDisplay"],
  mounted: function () {
    [
      document.querySelector("#no-like" + this.id + ""),
      document.querySelector("#yes-like" + this.id + ""),
    ].forEach((element) => {
      element.addEventListener("click", () => {
        document
          .querySelector("#yes-like" + this.id + "")
          .classList.toggle("hidden");
        document
          .querySelector("#no-like" + this.id + "")
          .classList.toggle("hidden");

        let data = [];
        let tabTempo = [];
        if (localStorage.getItem(this.type))
          tabTempo = JSON.parse(localStorage.getItem(this.type));
        if (!tabTempo.includes(this.url)) {
          tabTempo.push(this.url);
        } else {
          tabTempo.splice(tabTempo.indexOf(this.url), 1);
        }

        if (typeof tabTempo == "object")
          tabTempo.map((x) => {
            data.push(x);
          });
        localStorage.setItem(this.type, JSON.stringify(data));
      });
    });

    if (localStorage.getItem(this.type)) {
      let tabTempo = [];
      tabTempo = JSON.parse(localStorage.getItem(this.type));

      if (tabTempo.some((e) => this.id.charAt(0) + this.substr(e) == this.id)) {
        document
          .querySelector("#no-like" + this.id + "")
          .classList.add("hidden");
        document
          .querySelector("#yes-like" + this.id + "")
          .classList.remove("hidden");
      }
    }
  },
  methods: {
    substr: function (data) {
      return data.substring(data.lastIndexOf("/") + 1);
    },
  },
};
</script>

<style scoped>
.like {
  right: 18px;
  top: 50%;
  transform: translateY(-50%);
}
.like-chara {
  right: 18px;
  bottom: 15px;
}
</style>
