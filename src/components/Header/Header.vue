<template>
  <header class="header">
    <div class="container header-inner">
      <a href="#" class="logo">Movie Catalog</a>
      <input
          v-model="searchQuery"
          @input="handleInput"
          class="search"
          type="search"
          placeholder="Enter the name of the movie "
      >
      <span class="static-user">Alexander Borisenko</span>
    </div>
  </header>
  <main class="main">
    <div class="container">
      <div class="search-info">
        <h3
            v-if="searchQuery.length >= 3"
        >
          You searched for: {{ searchQuery }}, {{ totalResults }} result found
        </h3>
        <h3 v-else>Enter something in the search to get results</h3>
      </div>
      <div class="movies">
        <div v-for="movie in movies" :key="movie.imdbID" class="movie-card">
          <img class="movie-poster" v-if="movie.Poster !== 'N/A'" :src="movie.Poster" :alt="movie.Title" />
          <p v-else>No Poster Available</p>
          <div class="movie-info">
            <h3 class="movie-title">Name: {{ movie.Title }}</h3>
            <p>Year: {{ movie.Year }}</p>
            <p>imdbID: {{ movie.imdbID }}</p>
            <p>Type: {{ movie.Type }}</p>
          </div>
        </div>
      </div>
      <div v-if="movies.length > 0" class="pagination">
        <span @click="changePage(page - 1)"> < </span>
        <span
            class="page"
            :key="pageNum"
            v-for="pageNum in displayedPages"
            :class="{'current-page': page === pageNum}"
            @click="changePage(pageNum)"

        >{{ pageNum }}</span>
        <span @click="changePage(page + 1)"> > </span>
      </div>
    </div>
  </main>
</template>

<script setup>
import { computed, ref } from "vue";
  import axios from "axios";
  import { API_KEY, API_URL } from "@/constants.js";

  const searchQuery = ref('');
  const movies = ref([]);
  const totalResults = ref(0);
  const page = ref(1);
  const totalPages = ref(0);

  const handleInput = async (ev) => {
    const input = ev.target.value;

    if (input.trim().length >= 3) {
      await fetchMovies(input);
    } else {
      movies.value = [];
      totalResults.value = 0;
    }
  };

  const changePage = async (pageNumber) => {
    if (pageNumber >= 1 && pageNumber <= totalPages.value) {
      page.value = pageNumber;
      await fetchMovies(searchQuery.value);
    }
  };


  const fetchMovies = async (query) => {
    try {
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
    }
  };

  const displayedPages = computed(() => {
    const current = page.value;
    const total = totalPages.value;
    const range = 10;

    const start = Math.max(1, current - Math.floor(range / 2));
    const end = Math.min(total, start + range - 1);

    return Array.from({ length: end - start + 1 }, (_, i) => start + i);
  });

</script>

<style scoped>
  .header {
    background: #BADAED;
    padding: 20px 0;
  }

  .header-inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo {
    font-size: 34px;
    font-weight: bold;
  }

  .search {
    flex-grow: 1;
    margin: 0 25px;
    padding: 8px 5px;
    border: none;
  }

  .static-user {
    padding: 7px 0 7px 40px;
    background: url("../../assets/icons/ic-user.png") 0 0 no-repeat;
    position: relative;
  }

  .static-user:after {
    content: '';
    position: absolute;
    top: 7px;
    right: -18px;
    background: url("../../assets/icons/ic-arrow-down.png") 0 0 no-repeat;
    width: 15px;
    height: 15px;
  }

  .main {
    margin: 20px 0;
  }

  .search-info {
    font-size: 22px;
  }

  .movies {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 40px;
  }

  .movie-card {
    width: 21%;
    padding: 15px;
    font-size: 16px;
    display: flex;
    flex-direction: column;
  }

  .movie-poster {
    max-width: 100%;
    height: 330px;
    display: block;
  }

  .movie-info {
    padding: 10px 20px;
  }

  .movie-title {
    font-weight: 400;
    font-size: inherit;
  }

  .movie-card p {
    margin: 10px 0;
  }

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
