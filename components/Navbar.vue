<template>
  <div>
    <b-navbar
      fixed="top"
      toggleable="lg"
      type="dark"
      class="py-3"
      variant="oxford-blue"
    >
      <b-navbar-brand><NuxtLink to="/">Movie App</NuxtLink></b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-mavbar-nav class="ml-auto">
          <b-nav-form @submit.stop.prevent="searchForResults">
            <b-form-input
              size="sm"
              class="mr-sm-2 bg-oxford-blue"
              placeholder="Search"
              type="search"
              autocomplete="off"
              v-model="searchQuery"
              @focus="onFocus"
              @blur="onBlur"
            ></b-form-input>

            <b-button size="sm" class="my-2 my-sm-0" type="submit"
              >Search</b-button
            >
          </b-nav-form>
          <b-list-group
            class="position-absolute mt-2"
            v-show="moviesFinded.length > 0 && showDataList"
          >
            <NuxtLink
              :to="`/movie/${movie.id}`"
              v-for="movie in moviesFinded"
              :key="movie.id"
            >
              <b-list-group-item variant="oxford-blue">{{
                movie.title
              }}</b-list-group-item>
            </NuxtLink>
          </b-list-group>
        </b-mavbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <!-- Using 'button-content' slot -->
            <template #button-content>
              <em>Categories</em>
            </template>
            <b-dropdown-item
              varient="dark"
              :href="`/categories/${category.id}`"
              v-for="category in categories"
              :key="category.id"
              >{{ category.name }}
              <sup class="text-info"
                ><small
                  ><i>Id: {{ category.id }}</i></small
                ></sup
              ></b-dropdown-item
            >
          </b-nav-item-dropdown>
          <b-nav-item><NuxtLink to="/popular">Popular</NuxtLink></b-nav-item>
          <b-nav-item
            ><NuxtLink to="/top_rated">Top Rated</NuxtLink></b-nav-item
          >
          <b-nav-item><NuxtLink to="/upcoming">Uncoming</NuxtLink></b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Navbar",
  data() {
    return {
      categories: [],
      searchQuery: "",
      moviesFinded: [],
      showDataList: false,
    };
  },
  created() {
    this.getMovies();
  },
  watch: {
    searchQuery(newQuery) {
      if (newQuery.length > 2) {
        this.getQueryFinded(newQuery);
      }
    },
  },
  methods: {
    onFocus: function () {
      this.showDataList = true;
    },
    onBlur: function () {
      setTimeout(() => (this.showDataList = false), 500);
    },
    async getMovies() {
      try {
        const data = await axios.get(
          "https://api.themoviedb.org/3/genre/movie/list?api_key=c3141c935b054ff7144cf696643fe063"
        );
        this.categories = data.data.genres;
        // console.log(data);
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
    async getQueryFinded(val) {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/search/movie?api_key=c3141c935b054ff7144cf696643fe063&query=${val}`
        );
        this.moviesFinded = data.data.results;
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
    searchForResults(){
      this.$router.push("/search?query="+this.searchQuery);
    }
  },
};
</script>