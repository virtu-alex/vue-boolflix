<template>
  <div id="app">
    <SearchBar placeholder="Cerca..." @search="getQuerys" />
    <main>
      <div class="container m-0">
        <div class="row">
          <div class="col">
            <h3 class="ps-5">Movies list</h3>
            <ul class="d-flex flex-direction-column">
              <li class="m-3"></li>
              <div
                class="card-container position-relative px-3"
                v-for="movie in productions.mappedMovies"
                :key="movie.id"
              >
                <div class="card">
                  <img
                    v-if="movie.poster_path === null"
                    src="./assets/noimage.jpeg"
                    alt=""
                  />
                  <img
                    v-else
                    :src="`https://image.tmdb.org/t/p/w342/${movie.poster_path}`"
                    :alt="movie.original"
                  />
                </div>
                <div class="overlay">
                  <li><strong>Movie Title - </strong> {{ movie.original }}</li>
                  <li>
                    <strong>Original Movie Title - </strong>
                    {{ movie.original_heading }}
                  </li>
                  <li>
                    <strong>Movie Lenguage - </strong>
                  </li>
                  <li>
                    <img
                      class="flag mx-1"
                      :src="require(`./assets/flags/${movie.flagPath}.png`)"
                      :alt="movie.original"
                    />{{ movie.original_language }}
                    <FontAwesomeIcon
                      v-for="index in getStarsVote(movie.vote_average)"
                      :key="index + 10"
                      icon="fa-solid fa-star"
                    />
                    <FontAwesomeIcon
                      v-for="index in 5 - getStarsVote(movie.vote_average)"
                      :key="index"
                      icon="fa-regular fa-star"
                    />
                    <strong>Avarage Movie Vote - </strong>
                    {{ movie.vote_average }}
                  </li>
                </div>
              </div>
            </ul>
          </div>
        </div>
        <div class="row mt-5">
          <div class="col">
            <h3 class="ps-5">TV Series list</h3>
            <ul class="d-flex flex-direction-column">
              <li class="m-3"></li>
              <div
                class="card-container position-relative px-3"
                v-for="serie in productions.mappedSeries"
                :key="serie.id"
              >
                <div class="card">
                  <img
                    v-if="serie.poster_path === null"
                    src="./assets/noimage.jpeg"
                    alt=""
                  />
                  <img v-else
                    :src="`https://image.tmdb.org/t/p/w342/${serie.poster_path}`"
                    :alt="serie.original"
                  />
                </div>
                <div class="overlay">
                  <li><strong>Serie Title - </strong> {{ serie.original }}</li>
                  <li>
                    <strong>Original Serie Title - </strong>
                    {{ serie.original_heading }}
                  </li>
                  <li>
                    <strong>Serie Lenguage - </strong>
                  </li>
                  <li>
                    <img
                      class="flag mx-1"
                      :src="require(`./assets/flags/${serie.flagPath}.png`)"
                      alt=""
                    />
                    <FontAwesomeIcon
                      v-for="index in getStarsVote(serie.vote_average)"
                      :key="index + 20"
                      icon="fa-solid fa-star"
                    />
                    <FontAwesomeIcon
                      v-for="index in 5 - getStarsVote(serie.vote_average)"
                      :key="index"
                      icon="fa-regular fa-star"
                    />

                    <strong>Avarage Serie Vote - </strong>
                    {{ serie.vote_average }}
                  </li>
                </div>
              </div>
            </ul>
          </div>
        </div>
      </div>
    </main>
    <footer></footer>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
/* import the fontawesome core */
import { library } from "@fortawesome/fontawesome-svg-core";

/* import font awesome icon component */
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

/* import specific icons */
import { faStar } from "@fortawesome/free-regular-svg-icons";
import { faStar as faStarFull } from "@fortawesome/free-solid-svg-icons";
library.add(faStar, faStarFull);
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
    getStarsVote(fullVote) {
      return Math.ceil(fullVote / 2);
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
 **/

<style lang="scss">
@import "~bootstrap/scss/bootstrap";

.flag {
  width: 25px;
  height: 25px;
}

ul {
  list-style: none;
}

body {
  background-color: black;
  color: white;
  font-family: "League Gothic", sans-serif;
  font-family: "Stick No Bills", sans-serif;
}

.card {
  max-width: 150px;
}

.card-container {
  width: 200px;
}

.overlay {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  height: 100%;
  width: 100%;
  opacity: 0;
  transition: 0.5s ease;
  background-color: black;
}

.card-container:hover .overlay {
  opacity: 1;
  scale: 1.05;
}
</style>
