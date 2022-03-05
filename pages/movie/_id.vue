<template>
  <b-container>
    <b-button variant="outline-red-crayola" @click="goBack()"> Back </b-button>
    <h1>{{ movieDetails.title }}</h1>
    <p>{{ movieDetails.overview }}</p>
  </b-container>
</template>

<script>
import axios from "axios";
export default {
  props: ["movie"],
  data() {
    return {
      id: 0,
      movieDetails: {},
    };
  },
  created() {
    this.id = this.$route.params.id;
    this.getMovie();
  },
  methods: {
    async getMovie() {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.id}?api_key=c3141c935b054ff7144cf696643fe063`
        );
        this.movieDetails = data.data;
        // console.log(data);
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
    goBack(){
      window.history.length > 1 ? this.$router.go(-1) : this.$router.push('/')
    }
  },
};
</script>