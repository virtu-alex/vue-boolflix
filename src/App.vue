<template>
  <div id="app">
    <header>
      <SearchBar placeholder="Cerca..." @search="getQuerys" />
    </header>
    <main>
      <BaseMain :movies="movies" :series="series" />
    </main>
    <footer></footer>
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import BaseMain from "./components/BaseMain.vue";

export default {
  name: "App",
  components: {
    SearchBar,
    BaseMain,
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
