<template>
  <div v-if="totalPages > 1" class="pagination">
    <span @click="changePage(currentPage - 1)"> < </span>
    <span
        class="page"
        :key="pageNum"
        v-for="pageNum in displayedPages"
        :class="{ 'current-page': currentPage === pageNum }"
        @click="changePage(pageNum)"
    >{{ pageNum }}</span>
    <span @click="changePage(currentPage + 1)"> > </span>
  </div>
</template>

<script setup>
  import { computed } from 'vue';

  const props = defineProps({
    currentPage: {
      type: Number,
      required: true,
    },
    totalPages: {
      type: Number,
      required: true,
    }
  });

  const emits = defineEmits(['changePage']);

  const changePage = pageNumber => {
    if (pageNumber >= 1 && pageNumber <= props.totalPages) {
      emits('changePage', pageNumber);
    }
  };

  const displayedPages = computed(() => {
    const current = props.currentPage;
    const total = props.totalPages;
    const range = 10;

    const start = Math.max(1, current - Math.floor(range / 2));
    const end = Math.min(total, start + range - 1);

    return Array.from({ length: end - start + 1 }, (_, i) => start + i);
  });
</script>

<style scoped>
  .pagination {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }

  .page {
    margin: 0 8px;
    font-size: 20px;
    cursor: pointer;
  }

  .current-page {
    font-weight: bold;
  }
</style>