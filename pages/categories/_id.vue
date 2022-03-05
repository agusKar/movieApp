<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <h2>{{ type }} movies</h2>
        <ListMovies :movies="movieByType"/>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      id: "",
      movieByType: [],
    };
  },
  created() {
    this.id = this.$route.params.id;
    this.getType();
  },
  methods: {
    async getType() {
      try {
        const data = await axios.get(`https://api.themoviedb.org/3/discover/movie?api_key=c3141c935b054ff7144cf696643fe063&with_genres=${this.id}`)
        this.movieByType = data.data.results;
        // console.log(dataExample);
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
  },
};
</script>