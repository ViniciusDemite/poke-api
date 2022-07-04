<template>
  <HeaderSection>
    <SearchBar v-model:search="search" @searchPokemon="getPokemon" />
  </HeaderSection>

  <PokemonsSection :total="total">
    <div class="d-flex align-content-center justify-space-between mb-4">
      <PokemonPrevNext
        v-model:prev="prev"
        v-model:next="next"
        @nextPage="nextPagePokemons"
        @prevPage="prevPagePokemons"
      />
    </div>

    <div class="row gx-4 gy-5">
      <PokemonCard
        v-for="pokemon in pokemonsSlice"
        :key="pokemon.id"
        :pokemon="pokemon"
        @infoPokemon="showPokemon"
      />
    </div>
  </PokemonsSection>
</template>

<script>
import HeaderSection from "./components/HeaderSection.vue";
import SearchBar from "./components/SearchBar.vue";
import PokemonsSection from "./components/Pokemons/PokemonsSection.vue";
import PokemonCard from "./components/Pokemons/PokemonCard.vue";
import PokemonPrevNext from "./components/Pokemons/PokemonPrevNext.vue";

// TODO fazer componente de modal para mostrar resultado de pesquisa e informações detalhadas do pokemon

const defaultPerPage = 30;

export default {
  name: "App",

  components: {
    HeaderSection,
    SearchBar,
    PokemonsSection,
    PokemonCard,
    PokemonPrevNext,
  },

  data() {
    return {
      allPokemons: [],
      pokemonsSlice: [],
      pokemonsNamesList: [],
      total: 0,
      perPage: defaultPerPage,
      offset: 0,
      limit: defaultPerPage,
      prev: false,
      next: true,
      search: "",
      searchedPokemon: {},
    };
  },

  mounted() {
    this.getAllPokemons();
  },

  watch: {
    search: function () {
      const pokemonsList = this.allPokemons.filter((pokemon) =>
        pokemon.name.includes(this.search.toLocaleLowerCase())
      );

      this.pokemonsNamesList = pokemonsList;
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
      this.pokemonsSlice = [];

      if (this.limit > this.total) this.limit = this.total;

      console.log(`Offset: ${this.offset}`, `Limit: ${this.limit}`);

      // FIXME Retornar Promise.all
      for (let offset = this.offset; offset < this.limit; offset++) {
        const pokemon = this.allPokemons[offset];
        const { data } = await this.axios.get(pokemon.url);

        this.pokemonsSlice.push(data);
      }

      console.log("Números de pokemons: ", this.pokemonsSlice.length);
    },

    async getPokemon() {
      const { data } = await this.axios.get(
        `https://pokeapi.co/api/v2/pokemon/${this.search}`
      );

      this.searchPokemon = data;

      //FIXME tratar quando não for digitado o nome completo do pokemon
    },

    async nextPagePokemons() {
      if (this.limit >= this.total) return;

      this.offset = this.limit + 1;
      this.limit += this.perPage + 1;

      await this.getPokemonsSlice();

      this.prev = this.hasPrevElements();
      this.next = this.hasNextElements();
    },

    async prevPagePokemons() {
      if (this.offset === 0) return;

      this.limit -= this.perPage;
      this.offset -= this.limit;

      await this.getPokemonsSlice();

      this.prev = this.hasPrevElements();
      this.next = this.hasNextElements();
    },

    hasPrevElements() {
      return this.offset !== 0;
    },

    hasNextElements() {
      return this.limit < this.total;
    },

    showPokemon(pokemon) {
      console.log(pokemon.name);
    },
  },
};
</script>

<style src="./assets/css/app.css" />
