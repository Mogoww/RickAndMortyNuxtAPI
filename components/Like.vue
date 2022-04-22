<template>
  <div>
    <!-- Script -->
    <script
      type="module"
      src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.esm.js"
    ></script>
    <script
      nomodule
      src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.js"
    ></script>
    <div class="cursor-pointer">
      <ion-icon
        :id="'no-like'+id"
        class="text-red-700"
        name="heart-outline"
      ></ion-icon>
      <ion-icon
        :id="'yes-like'+id"
        class="hidden text-red-700"
        name="heart-sharp"
      ></ion-icon>
    </div>
  </div>
</template>

<script>
export default {
  name: "Like",
  props:["id"],
  mounted: function () {
    // localStorage.setItem("authToken", "token");
    // console.log(localStorage);
    [
      document.querySelector("#no-like"+this.id +""),
      document.querySelector("#yes-like"+this.id +""),
    ].forEach((element) => {
      element.addEventListener("click", () => {
        document.querySelector("#yes-like"+this.id +"").classList.toggle("hidden");
        document.querySelector("#no-like"+this.id +"").classList.toggle("hidden");
        console.log("mfkjgmgjf");
      });
    });
  },
};
</script>
