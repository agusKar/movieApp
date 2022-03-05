<template>
  <b-container>
    <b-row align-h="center" class="list-movies" v-if="movies">
      <b-col lg="4" sm="6" v-for="movie in movies" :key="movie.id">
        <CardMovie :movie="movie" />
      </b-col>
    </b-row>
    <b-row v-else>
      <h4>We dont find any movies for you. Try later.</h4>
    </b-row>
  </b-container>
</template>

<script>
export default {
  props: ["movies"],
};
</script>