<template>
  <div>
    <b-carousel
      id="carousel-1"
      :interval="6000"
      controls
      fade
      indicators
      background="#ababab"
      style="text-shadow: 1px 1px 2px #333; max-height: 450px"
    >
      <!-- Slide with blank fluid image to maintain slide aspect ratio -->
      <b-carousel-slide
        v-for="movie in lastMovies"
        :key="movie.id"
        :caption="movie.title"
        :img-src="`http://image.tmdb.org/t/p/w1280/${movie.poster_path}`"
        img-alt="Blank image"
        class="img_carousel"
      >
        <p>{{ movie.overview }}</p>
        <NuxtLink
          :to="`movie/${movie.id}`"
          class="btn btn-outline-red-crayola mb-3"
          >See details</NuxtLink
        >
      </b-carousel-slide>
    </b-carousel>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      lastMovies: [],
    };
  },
  created() {
    this.getLastMovies();
  },
  methods: {
    async getLastMovies() {
      try {
        let data = await axios(
          "https://api.themoviedb.org/3/trending/movie/week?api_key=c3141c935b054ff7144cf696643fe063"
        );
        this.lastMovies = data.data.results.slice(0, 5);
        console.log();
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
<style scoped>
/* CAROUSEL */

.carousel-fade .carousel-item.img_carousel {
  max-height: 450px;
}

.carousel-item.img_carousel {
  position: relative;
}
.carousel-item.img_carousel:after {
  content: "";
  background: linear-gradient(to bottom, #ffffff00 0%, var(--oxford-blue) 100%);
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  height: 100%;
  pointer-events: none;
}
</style>