<template>
  <div id="app">
    <BaseHeader @search-movies="getQueryMovies" />
  </div>
</template>

<script>
import BaseHeader from "./components/BaseHeader.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    BaseHeader,
  },
  data() {
    return {
      movies: [],
      querySearch: "",
    };
  },
  methods: {
    getQueryMovies() {
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie/?query=${this.querySearch}&api_key=6dace570cbb7c25a571ec1d0a83908b1`
        )
        .then((res) => {
          //estraggo da response i "results"
          this.movies = res.data["results"];
          console.log(this.movies);
        });
    },
    mounted(){
      this.getQueryMovies()
    }
  },
  
};
</script>

<style lang="scss">
</style>
