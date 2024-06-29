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
        <div v-for="(movie, index) in movies" :key="index" class="movie">
          <img class="movie-poster" v-if="movie.Poster !== 'N/A'" :src="movie.Poster" alt="Movie Poster" />
          <p v-else>No Poster Available</p>
          <h3>Name: {{ movie.Title }}</h3>
          <p>Year: {{ movie.Year }}</p>
          <p>imdbID: {{ movie.imdbID }}</p>
          <p>Type: {{ movie.Type }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
  import { ref } from "vue";
  import axios from "axios";
  import { API_KEY, API_URL } from "@/constants.js";

  const searchQuery = ref('');
  const movies = ref([]);
  const totalResults = ref(0);

  const handleInput = async (ev) => {
    const input = ev.target.value;

    if (input.trim().length >= 3) {
      await fetchMovies(input);
    }
  };

  const fetchMovies = async (query) => {
    try {
      const response = await axios.get(API_URL, {
        params: {
          apiKey: API_KEY,
          s: query
        }
      });

      if (response.data.Response === 'True') {
        movies.value = response.data.Search;
        totalResults.value = response.data.totalResults;
      } else {
        movies.value = [];
        totalResults.value = 0;
      }

      console.log(movies.value);
    } catch (e) {
      console.error('Error');
    }
  };

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
    font-size: 32px;
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
    font-size: 24px;
  }

  .movies {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    margin-top: 40px;
  }

  .movie {
    width: 21%;
  }

  .movie-poster {
    width: 200px;
    height: auto;
  }

</style>
