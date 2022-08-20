<template>
  <h2 class="title">
    <slot />
  </h2>
</template>

<script>
export default {
  name: "Title",
};
</script>

<style scoped>
.title {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 2.5rem;
  letter-spacing: 3px;
  border-bottom: 1px solid var(--light);
}
</style>
