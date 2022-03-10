<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <h2 class="mb-5">Search results:</h2>
        <ListMovies :movies="movies"/>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      movies: [],
      query: this.$route.query.query
    }
  },
  watch:{
     '$route.query.query': {
        handler: function(search) {
            this.query = this.$route.query.query;
            this.getQueryFinded(this.query);
        },
        // This will let Vue know to look inside the array
        deep: true,
        // Will fire as soon as the component is created
        immediate: true
      }
  },
  methods: {
    async getQueryFinded(val) {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/search/movie?api_key=c3141c935b054ff7144cf696643fe063&query=${val}`
        );
        this.movies = data.data.results;
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    }
  }
};
</script>