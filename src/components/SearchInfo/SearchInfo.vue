<template>
  <div class="search-info">
    <h3 v-if="searchQuery.length >= 3">
      You searched for: {{ searchQuery }}, {{ totalResults }} result found
    </h3>
    <h3 v-else>Enter something in the search to get results</h3>
  </div>
</template>

<script setup>
  defineProps({
    searchQuery: {
      type: String,
      required: true
    },
    totalResults: {
      type: Number,
      required: true
    }
  });
</script>

<style scoped>
  .search-info {
    font-size: 22px;
  }
</style>