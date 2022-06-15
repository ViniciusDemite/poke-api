<template>
  <HeaderSection>
    <SearchBar v-model:search="search" @searchPokemon="getPokemon" />
  </HeaderSection>

  <PokemonsSection>
    <div class="row gx-4 gy-5">
      <PokemonCard
        v-for="pokemon in pokemonsSlice"
        :key="pokemon.id"
        :pokemon="pokemon"
      />
    </div>
  </PokemonsSection>
</template>

<script>
import HeaderSection from "./components/HeaderSection.vue";
import SearchBar from "./components/SearchBar.vue";
import PokemonsSection from "./components/Pokemons/PokemonsSection.vue";
import PokemonCard from "./components/Pokemons/PokemonCard.vue";

// TODO fazer componente de modal para mostrar resultado de pesquisa e informações detalhadas do pokemon

export default {
  name: "App",

  components: {
    HeaderSection,
    SearchBar,
    PokemonsSection,
    PokemonCard,
  },

  data() {
    return {
      allPokemons: [],
      pokemonsSlice: [],
      total: 0,
      offset: 0,
      limit: 30,
      search: "",
      searchedPokemon: {},
    };
  },

  mounted() {
    this.getAllPokemons();
  },

  watch: {
    search: function () {
      /* const regex = new RegExp(this.search + ".+$", "i");
      const pokemonsList = this.allPokemons.filter(
        (pokemon) => pokemon.name.search(regex) !== 1
      );

      console.log(pokemonsList); */
    },
  },

  methods: {
    async getAllPokemons() {
      const {
        data: { results, count },
      } = await this.axios.get(
        "https://pokeapi.co/api/v2/pokemon?limit=100000&offset=0"
      );

      this.total = count;

      this.allPokemons = results;

      this.getPokemonsSlice();
    },

    async getPokemonsSlice() {
      for (let i = 0; i < this.limit; i++) {
        const pokemon = this.allPokemons[i];
        const { data } = await this.axios.get(pokemon.url);

        this.pokemonsSlice.push(data);
      }
    },

    async getPokemon() {
      const { data } = await this.axios.get(
        `https://pokeapi.co/api/v2/pokemon/${this.search}`
      );

      this.searchPokemon = data;

      //FIXME tratar quando não for digitado o nome completo do pokemon
    },
  },
};
</script>

<style src="./assets/css/app.css" />
