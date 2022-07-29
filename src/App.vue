<template>
  <div id="app">
    <SearchBar placeholder="Cerca..." @search="getQuerys" />
    <main>
      <div class="container">
        <h1>Informazioni titolo scelto</h1>
        <ul>
          <li><h3>LISTA FILM</h3></li>
          <li v-for="movie in productions.mappedMovies" :key="movie.id">
            <strong>Movie Title - </strong> {{ movie.original_heading }}
            <strong>Original Movie Title - </strong> {{ movie.original }}
            <strong>Movie Lenguage - </strong
            ><img
              class="flag"
              :src="require(`./assets/flags/${movie.flagPath}.png`)"
              alt=""
            />{{ movie.original_language }}
            <strong>Avarage Movie Vote - </strong> {{ movie.vote_average }}
          </li>
        </ul>
        <ul>
          <li><h3>LISTA SERIE TV</h3></li>
          <li v-for="serie in productions.mappedSeries" :key="serie.id">
            <strong>Serie Title - </strong> {{ serie.original_heading }}
            <strong>Original Serie Title - </strong> {{ serie.original }}
            <strong>Serie Lenguage - </strong
            ><img class="flag" :src="require(`./assets/flags/${serie.flagPath}.png`)" alt="" />
            <strong>Avarage Serie Vote - </strong> {{ serie.vote_average }}
          </li>
        </ul>
      </div>
    </main>
    <footer></footer>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";

export default {
  name: "App",
  components: {
    SearchBar,
  },
  data() {
    return {
      movies: [],
      series: [],
      baseUri: "https://api.themoviedb.org/3",
      apiKey: "6dace570cbb7c25a571ec1d0a83908b1",
      language: "it-IT",
    };
  },
  computed: {
    productions() {
      // const allProductions = [...this.movies, ...this.series];
      let mappedMovies = this.movies.map(this.mapProductions);
      let mappedSeries = this.series.map(this.mapProductions);
      return {
        mappedMovies,
        mappedSeries,
      };
    },
  },
  methods: {
    getQuerys(query) {
      //CALL FOR MOVIES
      if (!query) {
        this.movies = [];
        this.series = [];
        return;
      }
      const { apiKey, language } = this;
      const config = {
        params: {
          language,
          query,
          api_key: apiKey,
        },
      };
      this.fetchData("/search/tv", config, "series");
      this.fetchData("/search/movie", config, "movies");
    },
    mapProductions(production) {
      const { id, original_language, vote_average, poster_path } = production;
      const type = Object.keys(production).includes("original_name")
        ? "serie"
        : "movie";
      const originalHeading =
        type === "serie"
          ? production["original_name"]
          : production["original_title"];
      const original =
        type === "serie" ? production["name"] : production["title"];
      return {
        id,
        original_language,
        flagPath: ["it", "en"].includes(original_language) ? original_language : "nopic",
        vote_average,
        poster_path,
        original_heading: originalHeading,
        original,
        type,
      };
    },

    //endpoint: /search/tv/
    //config: https://axios-http.com/docs/req_config
    //target: Rappresenta un elemento dei data/stato del componente
    fetchData(endpoint, config, target) {
      axios.get(`${this.baseUri}${endpoint}`, config).then((res) => {
        this[target] = res.data.results;
      });
    },
  },
};
</script>
/** 
 1 tipo dell'informazione
 2 id 
 3 original_language 
 4 vote_average 
 5 original_name *series* 
 6 original_language
 7 original_title *movies* 
 8 name *series*
 9 title *movies* 
 10 poster_path
 */
<style lang="scss">
@import "~bootstrap/scss/bootstrap";

.flag {
  width: 25px;
  height: 25px;
}
</style>
