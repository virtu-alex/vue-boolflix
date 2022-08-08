<template>
  <header>
    <div class="container my-5 d-flex justify-content-end">
      <input type="text" v-model="keyword" class="me-4" />
      <button @click="select" role="button" class="btn btn-primary px-2 me-2">
        Search
      </button>
    </div>
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

<style lang="scss">
header {
  height: 80px;
  background-color: black;
}

.logo{
  height: 80px;
  width: 200px;
}&img{
  height: 80px;
  width: 150px;
}
</style>


titolo = title
titolo originale = original_title
lingua = original_language
voto = vote_average