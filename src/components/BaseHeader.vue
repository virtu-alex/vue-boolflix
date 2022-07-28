<template>
  <header>
    <input placeholder="Nome Film" type="text" />
    <button @click="$emit('searchMovies')">Search...</button>
  </header>
</template>

<script>
export default {
  name: "BaseHeader",
};
</script>

<style>
</style>