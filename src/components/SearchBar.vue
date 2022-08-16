<template>
  <header>
    <div class="container my-5 d-flex justify-content-center align-items-center">
      <img class="logo pe-3" src="../assets/bool-logo.png" alt="boolflix logo">

      <input
        :placeholder="placeholder"
        type="text"
        v-model.trim="keyword"
        class="me-4"
        @keyup.enter="emitSearch"
      />
      <button @click="emitSearch" role="button" class="btn btn-primary px-2 me-2">
        Search
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

<style>
.logo{
  height: 75px;
}
</style>