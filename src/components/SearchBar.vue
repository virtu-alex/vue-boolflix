<template>
  <header>
    <div class="container my-5 d-flex justify-content-center align-items-center">
      <img
        class="logo pe-3"
        src="../assets/bool-logo.png"
        alt="boolflix logo"
      />
      <ul class="d-flex m-0">
        <li><a href="#">Home</a></li>
        <li><a href="#">Movies</a></li>
        <li><a href="#">Series</a></li>
        <li><a href="#">My List</a></li>
      </ul>
      <input
        :placeholder="placeholder"
        type="text"
        v-model.trim="keyword"
        class="ms-5"
        @keyup.enter="emitSearch"
      />
      <button @click="emitSearch" role="button" class="btn btn-danger ms-1">
        Cerca...
      </button>
    </div>
  </header>
</template>

<script>
export default {
  name: "SearchBar",
  props: {
    placeholder: String,
  },
  data() {
    return {
      keyword: "",
    };
  },
  methods: {
    emitSearch() {
      this.$emit("search", this.keyword);
    },
  },
};
</script>

<style lang="scss">
.logo {
  height: 75px;
}
header li {
  padding-right: 20px;
  font-size: 1.8rem;
  & a {
    text-decoration: none;
    color: whitesmoke;
  }
}
</style>