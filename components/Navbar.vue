<template>
  <div>
    <b-navbar
      fixed="top"
      toggleable="lg"
      type="dark"
      class="py-3"
      variant="oxford-blue"
    >
      <b-navbar-brand href="#"><NuxtLink to="/">Movie App</NuxtLink></b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-mavbar-nav class="ml-auto">
          <b-nav-form>
            <b-form-input
              size="sm"
              class="mr-sm-2 bg-oxford-blue"
              placeholder="Search"
            ></b-form-input>
            <b-button size="sm" class="my-2 my-sm-0" type="submit"
              >Search</b-button
            >
          </b-nav-form>
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
              v-for="(category, index) in categories"
              :key="category.id"
              >{{ category.name }} <sup class="text-info"><small><i>Id: {{category.id}}</i></small></sup></b-dropdown-item
            >
          </b-nav-item-dropdown>
          <b-nav-item><NuxtLink to="/popular">Popular</NuxtLink></b-nav-item>
          <b-nav-item><NuxtLink to="/top_rated">Top Rated</NuxtLink></b-nav-item>
          <b-nav-item><NuxtLink to="/upcoming">Uncoming</NuxtLink></b-nav-item>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      categories: [],
    };
  },
  created() {
    this.getMovies();
  },
  methods: {
    async getMovies() {
      try {
        const data = await axios.get(
          'https://api.themoviedb.org/3/genre/movie/list?api_key=c3141c935b054ff7144cf696643fe063'
        );
        this.categories = data.data.genres;
        // console.log(data);
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    },
  },
};
</script>