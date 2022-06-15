<template>
  <div
    class="d-flex align-items-center justify-content-center position-relative"
  >
    <input
      type="search"
      placeholder="Pesquisar"
      class="form-control pe-5 search"
      :value="search"
      @input="$emit('update:search', $event.target.value)"
      @change="$emit('searchPokemon')"
    />
    <button
      type="button"
      class="btn btn-primary position-absolute end-0 search-button"
      @click="$emit('searchPokemon')"
    >
      <i class="fa-solid fa-magnifying-glass"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "SearchBar",

  emits: ["update:search", "searchPokemon"],

  props: {
    search: {
      type: String,
      default: "",
      required: true,
    },
  },

  methods: {},
};
</script>

<style scoped src="../assets/css/search-bar.css" />