<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <h2 class="mb-5">{{ type }} movies</h2>
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
      type: "",
      movieByType: [],
    };
  },
  created() {
    this.type = this.$route.params.type;
    this.getType();
  },
  methods: {
    async getType() {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.type}?api_key=c3141c935b054ff7144cf696643fe063`
        );
        this.movieByType = data.data.results;
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
  },
};
</script>