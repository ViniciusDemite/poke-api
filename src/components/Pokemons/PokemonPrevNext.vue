<template>
  <div class="prev-next">
    <button
      type="button"
      class="btn btn-primary button prev-button"
      :disabled="!prev"
      @click="$emit('prevPage')"
    >
      <i class="fa-solid fa-chevron-left"></i>
    </button>
    <button
      type="button"
      class="btn btn-primary next-button"
      v-if="next"
      @click="$emit('nextPage')"
    >
      <i class="fa-solid fa-chevron-right"></i>
    </button>
  </div>
</template>

<script>
export default {
  name: "PokemonPrevNext",

  props: {
    prev: {
      type: Boolean,
      required: true,
    },
    next: {
      type: Boolean,
      required: true,
    },
  },

  emits: ["prevPage", "nextPage"],
};
</script>

<style scoped src="../../assets/css/pokemon-prev-next.css" />