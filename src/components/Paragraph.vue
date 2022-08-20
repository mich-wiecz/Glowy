<template>
  <p class="paragraph">
    <slot />
  </p>
</template>

<script>
export default {
  name: "Paragraph",
};
</script>

<style scoped>
.paragraph {
  padding: 20px 30px;
  /* background: url('../binding_dark.png'); */
  background-color: var(--background);
  box-shadow: 0 0 8px 4px var(--secondary);
  color: #fff;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
