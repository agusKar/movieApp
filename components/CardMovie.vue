<template>
  <NuxtLink :to="`/movie/${movie.id}`">
    <div class="movie">
      <div
        class="movie-img"
        :style="`background: url('http://image.tmdb.org/t/p/w300/${movie.poster_path}')`"
      ></div>
      <div class="text-movie-cont">
        <h4>{{ movie.title }}</h4>
        <p>{{ movie.overview | cut(250) }}...</p>
        <b-button variant="outline-red-crayola">View details</b-button>
      </div>
    </div>
  </NuxtLink>
</template>

<script>
import cut from "vue-string-filter/dist/cut";
export default {
  filters: {
    cut,
  },
  props: ["movie"],
};
</script>
<style scoped>
.movie {
  background: -moz-linear-gradient(
    to bottom,
    #fff0 38%,
    #fff0 39%,
    #1e1b26e0 53%,
    #1e1b26 55%
  );
  /* FF3.6-15 */
  background: -webkit-linear-gradient(
    to bottom,
    #fff0 38%,
    #fff0 39%,
    #1e1b26e0 53%,
    #1e1b26 55%
  );
  /* Chrome10-25,Safari5.1-6 */
  background: linear-gradient(
    to bottom,
    #fff0 38%,
    #fff0 39%,
    #1e1b26e0 53%,
    #1e1b26 55%
  );
  /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#fff0', endColorstr='#1e1b26',GradientType=0 );
  /* IE6-9 */
  background-size: contain;
  background-size: cover;
  z-index: 1 !important;
  display: block;
  border-radius: 4px;
  margin-bottom: 1.6666em;
}
.movie:hover .movie-img {
  transform: scale(0.95);
}

.movie-img {
  width: 100%;
  height: 380px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  z-index: 111 !important;
  border-top-left-radius: 4px;
  border-top-right-radius: 4px;
  -webkit-mask-image: -webkit-gradient(
    linear,
    left top,
    left bottom,
    color-stop(0, black),
    color-stop(0.35, black),
    color-stop(0.5, black),
    color-stop(0.65, black),
    color-stop(0.85, rgba(0, 0, 0, 0.6)),
    color-stop(1, transparent)
  );
  position: relative;
  transition: 0.5s all;
}

.text-movie-cont {
  padding: 1em;
}
.movie h4 {
  font-size: 2em;
  font-weight: 400;
}

.movie p {
  font-size: 0.8em;
  font-weight: 400;
  color: #9b9b9b;
  line-height: 1.3;
}
</style>