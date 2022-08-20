<template>
  <g-link :to="to" class="link">
    <slot />
  </g-link>
</template>

<script>
export default {
  name: "Link",
  props: ["to"],
};
</script>

<style scoped>
.link {
  text-align: center;
  padding: 8px 16px;
  border-bottom: 1px solid var(--primary);
  text-decoration: none;
  color: #fff;
}

.link:hover {
  box-shadow: 0 0 5px 5px var(--primary);
}
</style>
