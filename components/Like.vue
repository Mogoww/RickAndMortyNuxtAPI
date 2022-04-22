<template>
  <div>
    <div class="cursor-pointer text-3xl flex items-center">
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
  props: ["id", "url", "type"],
  mounted: function () {
    // localStorage.setItem(this.type, "");
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
        console.log("mfkjgmgjf");

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
