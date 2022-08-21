<template>
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
                src="../assets/noimage.jpeg"
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
                  :src="require(`../assets/flags/${movie.flagPath}.png`)"
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
                src="../assets/noimage.jpeg"
                alt=""
              />
              <img
                v-else
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
                  :src="require(`../assets/flags/${serie.flagPath}.png`)"
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
</template>

<script>
/* import the fontawesome core */
import { library } from "@fortawesome/fontawesome-svg-core";

/* import font awesome icon component */
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

/* import specific icons */
import { faStar } from "@fortawesome/free-regular-svg-icons";
import { faStar as faStarFull } from "@fortawesome/free-solid-svg-icons";
library.add(faStar, faStarFull);
export default {
  name: "BaseMain",
  props: {
    movies: Array,
    series: Array,
  },
  components: {
    FontAwesomeIcon,
  },
  data() {
    return {};
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
  },
};
</script>

<style>
</style>