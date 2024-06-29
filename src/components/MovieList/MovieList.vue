<template>
  <div class="movies">
    <MovieCard
        v-for="movie in movies"
        :key="movie.imdbID"
        :movie="movie"
    />
  </div>
</template>

<script setup>
  import MovieCard from '@/components/MovieList/MovieCard/MovieCard.vue';

  defineProps({
    movies: {
      type: Array,
      required: true
    }
  });
</script>

<style scoped>
  .movies {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    margin-top: 40px;
  }
</style>