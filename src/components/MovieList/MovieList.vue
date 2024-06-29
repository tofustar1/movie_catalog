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
    gap: 10px 50px;
    margin: 40px 0;
  }

  @media screen and (max-width: 1000px) {
    .movies {
      gap: 10px 20px;
    }
  }

</style>