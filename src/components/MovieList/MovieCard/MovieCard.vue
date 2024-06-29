<template>
  <div class="movie-card">
    <img class="movie-poster" v-if="movie.Poster !== 'N/A'" :src="movie.Poster" :alt="movie.Title" />
    <p v-else>No Poster Available</p>
    <div class="movie-info">
      <h3 class="movie-title">Name: {{ movie.Title }}</h3>
      <p>Year: {{ movie.Year }}</p>
      <p>imdbID: {{ movie.imdbID }}</p>
      <p>Type: {{ movie.Type }}</p>
    </div>
  </div>
</template>

<script setup>
  defineProps({
    movie: {
      type: Object,
      required: true
    }
  });
</script>

<style scoped>
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
</style>