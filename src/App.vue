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
    pokemonsSlice: function () {
      if (this.pokemonsSlice.length === 0) return;

      this.hasPrevElements();
      this.hasNextElements();
    },
  },

  methods: {
    async getAllPokemons() {
      const {
        data: { results, count },
      } = await this.axios.get(
        "https://pokeapi.co/api/v2/pokemon?offset=0&limit=100000"
      );

      this.total = count;

      this.allPokemons = results;

      this.getPokemonsSlice();
    },

    async getPokemonsSlice() {
      this.pokemonsSlice = [];
      const promisedData = await this.getPokemonsSliceData();

      this.pokemonsSlice = promisedData.map((pokemon) => {
        return pokemon.data;
      });

      console.log(`Resultados: ${this.pokemonsSlice.length}`);
    },

    getPokemonsSliceData() {
      let promisedEvents = [];

      for (let offset = this.offset; offset < this.limit; offset++) {
        if (offset > this.allPokemons.length) break;

        const pokemon = this.allPokemons[offset];
        promisedEvents.push(this.axios.get(pokemon.url));
      }

      return Promise.all(promisedEvents);
    },

    async getPokemon() {
      const { data } = await this.axios.get(
        `https://pokeapi.co/api/v2/pokemon/${this.search}`
      );

      this.searchPokemon = data;

      //FIXME tratar quando não for digitado o nome completo do pokemon
    },

    nextPagePokemons() {
      if (this.limit === this.perPage) this.limit += 1;

      this.offset = this.limit;
      this.limit += this.perPage;

      this.getPokemonsSlice();
    },

    prevPagePokemons() {
      if (this.offset === this.perPage + 1) {
        this.offset -= 1;
        this.limit -= 1;
      }

      this.limit -= this.perPage;
      this.offset -= this.perPage;

      this.getPokemonsSlice();
    },

    hasPrevElements() {
      this.prev = this.offset !== 0;
    },

    hasNextElements() {
      this.next = this.limit < this.total;
    },

    showPokemon(pokemon) {
      console.log(pokemon.name);
    },
  },
};
</script>

<style src="./assets/css/app.css" />
