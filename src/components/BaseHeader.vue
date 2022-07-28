<template>
  <header>
    <input placeholder="Nome Film" type="text" v-model="select" />
    <button type="button" @click="keyword">Search...</button>
    <ul>
      <h1>Informazioni titolo scelto</h1>
      <li>1.Titolo - {{ title }}</li>
      <li>2.Titolo Originale - {{ original_title }}</li>
      <li>3.Lingua Originale - {{ original_lenguage }}</li>
      <li>4.Voto Medio - {{ vote_average }}</li>
    </ul>
  </header>
</template>

<script>
export default {
  name: "BaseHeader",
  data() {
    return {
      //salvo il valore dell'input che sarà dinamica in base al testo digitato
      select: "",
    };
  },
  props: {
    movies: Array,
    title: String,
    original_title: String,
    original_lenguage: String,
    vote_average: Number,
  },
  methods: {
    keyword() {
      this.$emit("search-movies", this.select);
      this.select = "";
    },
  },
};
</script>

<style>
</style>


titolo = title
titolo originale = original_title
lingua = original_language
voto = vote_average