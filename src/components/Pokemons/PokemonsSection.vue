<template>
  <div class="py-5">
    <div class="container">
      <h1 class="text-center mb-5 page-title">Pokemons</h1>

      <slot></slot>
    </div>

    <!-- <div class="navigation-container">
      <select class="per-page" v-model.number="perPage">
        <option v-for="v in select" :key="v">
          {{ v }}
        </option>
      </select>

      <div class="prev-next">
        <button
          type="button"
          class="button prev-button"
          v-if="prev"
          @click="prevPokemons()"
        >
          <i class="fa-solid fa-chevron-left"></i>
        </button>
        <button
          type="button"
          class="button next-button"
          @click="nextPokemons()"
        >
          <i class="fa-solid fa-chevron-right"></i>
        </button>
      </div>
    </div> -->

    <!-- <h4 class="pokemon__count">Total: {{ total }}</h4> -->
  </div>
</template>

<script>
export default {
  name: "PokemonsSection",

  data() {
    return {
      total: 0,
      prev: false,
      next: false,
      perPage: 30,
      offset: 0,
      limit: 30,
      select: [30, 60, 90, 120],
    };
  },

  watch: {
    perPage: function () {
      this.limit = this.perPage;

      this.getPokemons();
    },
  },

  methods: {
    nextPokemons() {
      this.offset = this.limit;
      this.limit += this.perPage;

      this.getPokemons();
    },

    prevPokemons() {
      this.offset =
        this.offset !== this.perPage + 1 ? this.offset - this.perPage : 0;
      this.limit -= this.perPage;

      this.getPokemons();
    },
  },
};
</script>

<style scoped src="../../assets/css/pokemons-section.css" />