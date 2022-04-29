<template>
  <div
    class="flex items-center cursor-pointer text-4xl"
    v-on:click="handleClick"
    @click="toggleActive = !toggleActive"
  >
    <div :class="{ hidden: toggleActive }">
      <ion-icon name="chevron-up-outline"></ion-icon>
    </div>

    <div :class="{ hidden: !toggleActive }">
      <ion-icon name="chevron-down-outline"></ion-icon>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      toggleActive: false,
    };
  },
  methods: {
    handleClick: function () {
      this.$emit("update:parent", this.toggleActive);
    },
  },
};
</script>
