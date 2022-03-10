<template>
  <b-container fluid class="p-0">
    <div class="movie-card">
      <div class="container-image">
        <b-img
          fluid
          :src="`http://image.tmdb.org/t/p/w300/${movieDetail.poster_path}`"
          alt="Image 1"
          class="cover"
        ></b-img>
      </div>
      <!-- end container image -->

      <div class="hero" :style="`background: url('http://image.tmdb.org/t/p/w1280/${movieDetail.poster_path}')`">
        <div class="details">
          <div class="title1">{{ movieDetail.title }}</div>

          <div class="title2">{{ movieDetail.tagline }}</div>

          <fieldset class="rating">
            <b-icon icon="star-fill" variant="warning"></b-icon>
            <b-icon icon="star-fill" variant="warning"></b-icon>
            <b-icon icon="star-fill" variant="warning"></b-icon>
            <b-icon icon="star-fill" variant="warning"></b-icon>
          </fieldset>

          <span class="likes"
            ><b-icon icon="heart-fill" variant="danger"></b-icon>
            {{ movieDetail.vote_count }} Likes</span
          >
        </div>
        <!-- end details -->
      </div>
      <!-- end hero -->

      <div class="description mt-5">
        <b-container>
          <b-row>
            <b-col>
              <b-badge
                pill
                variant="dark"
                v-for="genre in movieDetail.genres"
                :key="genre.id"
              >
                <NuxtLink :to="`/categories/${genre.id}`">{{
                  genre.name
                }}</NuxtLink>
              </b-badge>
            </b-col>
            <b-col cols="10">
              <p>
                {{ movieDetail.overview }}
              </p>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <div class="avatars">
                <b-avatar
                  v-for="cast in movieCredits"
                  :key="cast.id"
                  size="4em"
                  class="m-1"
                  :src="`http://image.tmdb.org/t/p/w185/${cast.profile_path}`"
                  v-b-tooltip.html
                  :title="`${cast.name} </br><strong class='text-red-crayola'>${cast.character}</strong>`"
                ></b-avatar></div
            ></b-col>
          </b-row>
        </b-container>
        <!-- end column2 -->
      </div>
      <!-- end description -->
    </div>
    <!-- end movie-card -->
  </b-container>
</template>

<script>
import axios from "axios";
export default {
  props: ["movie"],
  data() {
    return {
      id: 0,
      movieDetail: {},
      movieCredits: [],
    };
  },
  created() {
    this.id = this.$route.params.id;
    this.getMovie();
    this.getMovieCredits();
  },
  methods: {
    async getMovie() {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.id}?api_key=c3141c935b054ff7144cf696643fe063`
        );
        this.movieDetail = data.data;
        //console.log(this.movieDetail);
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
    async getMovieCredits() {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/movie/${this.id}/credits?api_key=c3141c935b054ff7144cf696643fe063`
        );
        this.movieCredits = data.data.cast;
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
    goBack() {
      window.history.length > 1 ? this.$router.go(-1) : this.$router.push("/");
    },
  },
};
</script>
<style scoped>
.movie-card {
  font: 14px/22px "Lato", Arial, sans-serif;
  color: #a9a8a3;
}

.movie-card .container-image img {
  max-height: 320px;
  box-shadow: 0px 0px 15px #191010;
}

.hero {
  height: 342px;
  margin: 0;
  position: relative;
  overflow: hidden;
  z-index: 1;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
}
.hero:after {
  content: "";
  background: linear-gradient(to bottom, #ffffff00 0%, var(--oxford-blue) 100%);
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  height: 100%;
  pointer-events: none;
  z-index: -1;
}

.cover {
  position: absolute;
  top: 80px;
  left: 40px;
  z-index: 2;
}

.details {
  padding: 190px 0 0 280px;
}
.details .title1 {
  color: white;
  font-size: 44px;
  margin-bottom: 13px;
  position: relative;
}
.details .title1 span {
  position: absolute;
  top: 3px;
  margin-left: 12px;
  background: #c4af3d;
  border-radius: 5px;
  color: #544c21;
  font-size: 14px;
  padding: 0px 4px;
}
.details .title2 {
  color: #c7c1ba;
  font-size: 23px;
  font-weight: 300;
  margin-bottom: 15px;
}
.details .likes {
  margin-left: 24px;
}

.description {
  bottom: 0px;
  height: 200px;
  font-size: 16px;
  line-height: 26px;
  color: #b1b0ac;
}

.column1 {
  padding-left: 50px;
  padding-top: 120px;
  width: 220px;
  float: left;
  text-align: center;
}

.column2 {
  padding-left: 41px;
  padding-top: 30px;
  margin-left: 20px;
  width: 480px;
  float: left;
}

.badge {
  padding: 0.5em;
  font-size: 0.9em;
  margin-left: 0.3em;
  margin-top: 0.3em;
}

/****** Style Star Rating Widget *****/
.rating {
  border: none;
  float: left;
}

.rating > input {
  display: none;
}

.rating > label:before {
  margin: 5px;
  margin-top: 0;
  font-size: 1em;
  font-family: FontAwesome;
  display: inline-block;
  content: "";
}

.rating > .half:before {
  content: "";
  position: absolute;
}

.rating > label {
  color: #ddd;
  float: right;
}
</style>