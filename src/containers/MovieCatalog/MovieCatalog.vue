<template>
  <Header v-model="searchQuery"/>
  <main class="main">
    <div class="container">
      <SearchInfo :searchQuery="searchQuery" :totalResults="totalResults" />
      <div v-if="isLoading">
        <Spinner />
      </div>
      <div v-else>
        <MovieList :movies="movies" />
      </div>
      <Pagination
          v-if="movies.length > 0"
          :currentPage="page"
          :totalPages="totalPages"
          @changePage="changePage"
      />
    </div>
  </main>
</template>

<script setup>
  import { ref, watch } from 'vue';
  import axios from 'axios';
  import { API_KEY, API_URL } from '@/constants.js';
  import Header from '@/components/Header/Header.vue'
  import SearchInfo from "@/components/SearchInfo/SearchInfo.vue";
  import MovieList from "@/components/MovieList/MovieList.vue";
  import Pagination from "@/components/Pagination/Pagination.vue";
  import Spinner from "@/components/Spinner/Spinner.vue";

  const searchQuery = ref('');
  const movies = ref([]);
  const totalResults = ref(0);
  const page = ref(1);
  const totalPages = ref(0);
  const isLoading = ref(false);

  watch(searchQuery, async (newValue) => {
    if (newValue.trim().length >= 3) {
      await fetchMovies(newValue);
    } else {
      movies.value = [];
      totalResults.value = 0;
    }
  });

  const fetchMovies = async (query) => {
    try {
      isLoading.value = true;
      const response = await axios.get(API_URL, {
        params: {
          apiKey: API_KEY,
          s: query,
          page: page.value
        }
      });
      if (response.data.Response === 'True') {
        movies.value = response.data.Search;
        totalResults.value = response.data.totalResults;
        totalPages.value = Math.ceil(totalResults.value / 10);
      } else {
        movies.value = [];
        totalResults.value = 0;
      }
    } catch (e) {
      console.error('Error');
    } finally {
      isLoading.value = false;
    }
  };

  const changePage = async (pageNumber) => {
    if (pageNumber >= 1 && pageNumber <= totalPages.value) {
      page.value = pageNumber;
      await fetchMovies(searchQuery.value);
    }
  };
</script>

<style scoped>
  .main {
    margin: 20px 0;
  }
</style>
