<template>
  <div>
      <Carousel />
      <b-container>
        <b-row class="mt-5">
          <b-col cols="3" v-for="category in categories" :key="category.id">
            <NuxtLink :to="`/categories/${category.id}`">
              <div class="box_category">
                <span>{{category.name}}</span>
              </div>
            </NuxtLink>
          </b-col>
        </b-row>
        <b-row class="mt-5">
          <ListMovies :movies="movieByType"/>
        </b-row>
      </b-container>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'IndexPage',
  data(){
    return{
      categories: [],
      movieByType: []
    }
  },
  created() {
    this.getType();
    this.getMovies();
  },
  methods: {   
    async getType() {
      try {
        const data = await axios.get(
          `https://api.themoviedb.org/3/movie/upcoming?api_key=c3141c935b054ff7144cf696643fe063`
        );
        this.movieByType = data.data.results;
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    }, 
    async getMovies() {
      try {
        const data = await axios.get(
          "https://api.themoviedb.org/3/genre/movie/list?api_key=c3141c935b054ff7144cf696643fe063"
        );
        this.categories = data.data.genres;
      } catch (error) {
        this.error = true;
        console.log(error);
      }
    }
  }
}
</script>
<style scoped>
  .box_category{
    box-shadow: rgba(0, 0, 0, 0.4) 0px 2px 4px, rgba(0, 0, 0, 0.3) 0px 7px 13px -3px, rgba(0, 0, 0, 0.2) 0px -3px 0px inset;
    padding: 1em;
    background-color: var(--smoky-black);
    color: var(--ivory);
    margin: .5em;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .box_category:hover{
    box-shadow: rgb(175, 35, 61, .25) 0px 30px 60px -12px inset, rgb(175, 35, 61, .3) 0px 18px 36px -18px inset;
  }
</style>