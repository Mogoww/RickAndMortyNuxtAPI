<template>
  <div>
    {{episode.episode}}
      card components
      {{episode}}
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  props: ["episode"],
};
</script>