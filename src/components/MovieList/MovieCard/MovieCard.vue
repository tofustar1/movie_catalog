<template>
  <div class="movie-card">
    <img
        class="movie-poster"
        v-if="movie.Poster !== 'N/A'" :src="movie.Poster" :alt="movie.Title"
    />
    <img
        v-else
        class="movie-poster"
        src="../../../assets/images/image-not-available.png"
        alt="image not-available"
    />
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
    width: calc(25% - 38px);
    padding: 15px;
    font-size: 16px;
    display: flex;
    flex-direction: column;
  }
  
  @media screen and (max-width: 1000px) {
    .movie-card {
      width: calc(33% - 11px);
    }
  }

  @media screen and (max-width: 748px) {
    .movie-card {
      width: calc(50% - 11px);
    }
  }

  @media screen and (max-width: 475px) {
    .movie-card {
      width: 100%;
    }
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