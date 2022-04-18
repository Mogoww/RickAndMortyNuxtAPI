<template>
  <div class="pagination">
    <div v-if="parseInt(pageNum) > 2">
      <n-link :to="'?page=' + 1">
        {{ 1 }}
      </n-link>
    </div>
    <div v-else>
      <n-link to=""> </n-link>
    </div>

    <div v-if="pageNum > 3">...</div>

    <div v-if="parseInt(pageNum) > 1">
      <n-link :to="'?page=' + (parseInt(pageNum) - 1)">
        {{ pageNum - 1 }}
      </n-link>
    </div>

    <n-link :to="'?page=' + parseInt(pageNum)" class="active">
      {{ pageNum }}
    </n-link>

    <div v-if="parseInt(pageNum) < parseInt(pageMax)">
      <n-link :to="'?page=' + sum()"> {{ sum() }} </n-link>
    </div>

    <div v-if="pageNum < pageMax - 2">...</div>

    <div v-if="parseInt(pageNum) < parseInt(pageMax) - 1">
      <n-link :to="'?page=' + parseInt(pageMax)"> {{ pageMax }} </n-link>
    </div>
    <div v-else>
      <n-link to=""> </n-link>
    </div>
  </div>
</template>

<script>
export default {
  props: ["pageNum", "pageMax"],
  data() {
    return {
      page: 1,
      error: false,
    };
  },
  methods: {
    sum() {
      return parseInt(this.pageNum) + parseInt(1);
    },
    comparate() {
      return parseInt(this.pageNum) < parseInt(this.pageMax);
    },
  },
  watch: {
    "$route.query"() {
      window.location.reload();
    },
  },
};
</script>

<style scoped>
.pagination {
  display: flex;
  align-items: center;
  justify-content: center;
}

.pagination a {
  color: black;
  float: left;
  padding: 8px 16px;
  text-decoration: none;
}

.pagination a.active {
  background-color: #4caf50;
  color: white;
  border-radius: 5px;
}

.pagination a:hover:not(.active) {
  background-color: #ddd;
  border-radius: 5px;
}
</style>
