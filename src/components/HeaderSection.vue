<template>
  <header class="py-2 header">
    <div class="container">
      <slot></slot>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderSection",
};
</script>

<style scoped src="../assets/css/header.css" />