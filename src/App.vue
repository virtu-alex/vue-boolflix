<template>
  <div id="app">
    <!-- <BaseHeader @search-movies="getQueryMovies" /> -->
    <header>
      <div class="container my-5">
        <input type="text" v-model="keyword" class="me-4" />
        <button
          @click="getQuerys"
          role="button"
          class="btn btn-primary px-2 me-2"
        >
          Search
        </button>
      </div>
    </header>
    <main>
      <div class="container">
        <h1>Informazioni titolo scelto</h1>
        <ul>
          <li v-for="movie in movies" :key="movie.id">
            <strong>Movie Title - </strong> {{ movie.title }} -
            <strong>Original Movie Title - </strong> {{ movie.original_title }}
            <strong>Movie Lenguage - </strong>{{ movie.original_language }}
            <strong>Avarage Movie Vote - </strong> {{ movie.vote_average }}
          </li>
          <li v-for="serie in series" :key="serie.id">
            <strong>Serie Title - </strong> {{ serie.name }} -
            <strong>Original Serie Title - </strong> {{ serie.original_name }}
            <strong>Serie Lenguage - </strong>{{ serie.original_language }}
            <strong>Avarage Serie Vote - </strong> {{ serie.vote_average }}
          </li>
        </ul>
      </div>
    </main>
    <footer></footer>
  </div>
</template>

<script>
// import BaseHeader from "./components/BaseHeader.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    // BaseHeader,
  },
  data() {
    return {
      movies: [],
      series: [],
      keyword: "",
      apiKey: "6dace570cbb7c25a571ec1d0a83908b1",
    };
  },
  methods: {
    getQuerys() {
      axios
        .get(
          `https://api.themoviedb.org/3/search/tv/?query=${this.keyword}&api_key=${this.apiKey}`
        )
        .then((res) => {
          //estraggo da response i "results" delle serie TV
          this.series = res.data["results"].map((result) => {
            const { id, name, original_name, original_language, vote_average } =
              result;
            return {
              id,
              name,
              original_name,
              original_language,
              vote_average,
            };
          });
        });
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie/?query=${this.keyword}&language=it-IT&api_key=${this.apiKey}`
        )
        .then((res) => {
          //estraggo da response i "results" dei films
          this.movies = res.data["results"].map((result) => {
            const {
              id,
              title,
              original_title,
              original_language,
              vote_average,
            } = result;
            return {
              id,
              title,
              original_title,
              original_language,
              vote_average,
            };
          });
        });
    },
  },
  mounted() {
    this.getQuerys();
  },
};
</script>

<style lang="scss">
@import "~bootstrap/scss/bootstrap";
</style>
