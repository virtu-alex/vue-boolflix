//* aggiungo la copertina del film o della serie tv (ho solo la parte finale dell'url) ??dimensioni diverse?? 
//* URL DI BASE https://image.tmdb.org/t/p/ + ??DIMENSIONI??

<template>
  <div id="app">
    <SearchBar placeholder="Cerca..." @search="getQuerys" />
    <main>
      <div class="container m-0">
        <h1>Informazioni titolo scelto</h1>
        <ul class="d-flex flex-direction-column">
          <li class="m-4"><h3>LISTA FILM</h3></li>
          <li v-for="movie in productions.mappedMovies" :key="movie.id">
            <strong>Movie Title - </strong> {{ movie.original_heading }}
            <strong>Original Movie Title - </strong> {{ movie.original }}
            <strong>Movie Lenguage - </strong
            ><img
              class="flag"
              :src="require(`./assets/flags/${movie.flagPath}.png`)"
              alt=""
            />{{ movie.original_language }}
            <img
              :src="`https://image.tmdb.org/t/p/w342/${movie.poster_path}`"
              alt=""
            />
            <FontAwesomeIcon icon="fa-regular fa-star" />
            <strong>Avarage Movie Vote  - </strong> {{ movie.vote_average }}
          </li>
          <li></li>
        </ul>
        <ul class="d-flex flex-direction-column">
          <li class="m-4"><h3>LISTA SERIE TV</h3></li>
          <li v-for="serie in productions.mappedSeries" :key="serie.id">
            <strong>Serie Title - </strong> {{ serie.original_heading }}
            <strong>Original Serie Title - </strong> {{ serie.original }}
            <strong>Serie Lenguage - <i class="fa-regular fa-star"></i> <i class="fa-regular fa-star"></i> </strong
            ><img
              class="flag"
              :src="require(`./assets/flags/${serie.flagPath}.png`)"
              alt=""
            />
            <img
              :src="`https://image.tmdb.org/t/p/w342/${serie.poster_path}`"
              alt=""
            />

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
/* import the fontawesome core */
import { library } from '@fortawesome/fontawesome-svg-core'

/* import font awesome icon component */
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

/* import specific icons */
import { faStar } from '@fortawesome/free-regular-svg-icons'
library.add(faStar)
export default {
  name: "App",
  components: {
    SearchBar,
    FontAwesomeIcon,
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
        flagPath: ["it", "en"].includes(original_language)
          ? original_language
          : "nopic",
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
ul{
  list-style: none;
}
</style>
