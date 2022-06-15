<template>
  <div class="col-lg-4 col-md-2 col-12">
    <article class="py-3 px-2 pokemon-card">
      <figure class="m-0">
        <img
          :src="pokemon.sprites.other.dream_world.front_default"
          alt="Imagem do pokemon"
          class="image"
        />
      </figure>
      <h2 class="text-center text-capitalize my-4">{{ pokemon.name }}</h2>
      <div class="d-flex align-content-center justify-content-center">
        <span
          v-for="t in pokemon.types"
          :key="t.slot"
          class="
            d-flex
            align-content-center
            justify-content-center
            text-center text-capitalize
            p-2
            type
          "
          :style="`background-color: ${colors[t.type.name]}`"
        >
          {{ t.type.name }}
        </span>
      </div>
    </article>
  </div>
</template>

<script>
export default {
  name: "PokemonCard",

  data() {
    return {
      colors: {},
    };
  },

  props: {
    pokemon: {
      type: Object,
      required: true,
    },
  },

  mounted() {
    this.colors = {
      normal: "#A8A77A",
      fire: "#EE8130",
      water: "#6390F0",
      electric: "#F7D02C",
      grass: "#7AC74C",
      ice: "#96D9D6",
      fighting: "#C22E28",
      poison: "#A33EA1",
      ground: "#E2BF65",
      flying: "#A98FF3",
      psychic: "#F95587",
      bug: "#A6B91A",
      rock: "#B6A136",
      ghost: "#735797",
      dragon: "#6F35FC",
      dark: "#705746",
      steel: "#B7B7CE",
      fairy: "#D685AD",
    };
  },
};
</script>

<style scoped src="../../assets/css/pokemon-card.css" />