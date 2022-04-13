<template>
  <div>
    <img :src="character.image" />
    {{ character.name }}
  </div>
</template>

<style>
</style>

<script>
export default {
  props: ["character"],
};
</script>