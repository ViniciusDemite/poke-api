<template>
  <HeaderSection>
    <SearchBar v-model:search="search" @searchPokemon="getPokemon" />
  </HeaderSection>

  <div class="container">
    <h1 class="text-center my-4 page-title">Pokemons</h1>

    <PokemonsContainer />
  </div>
</template>

<script>
import PokemonsContainer from "./components/PokemonsContainer.vue";
import HeaderSection from "./components/HeaderSection.vue";
import SearchBar from "./components/SearchBar.vue";

// TODO fazer componente de modal para mostrar resultado de pesquisa e informações detalhadas do pokemon

export default {
  name: "App",

  components: {
    HeaderSection,
    SearchBar,
    PokemonsContainer,
  },

  data() {
    return {
      search: "",
      searchedPokemon: {},
    };
  },

  watch: {},

  methods: {
    async getPokemon() {
      const { data } = await this.axios.get(
        `https://pokeapi.co/api/v2/pokemon/${this.search}`
      );

      this.searchPokemon = data;
    },
  },
};
</script>

<style src="./assets/css/app.css" />
