<template>
  <footer class="footer">
    <p>&copy; {{ new Date().getFullYear() }} | All Rights Reserved</p>
  </footer>
</template>

<script>
export default {
  name: "Footer",
};
</script>

<style scoped>
.footer {
  padding: 4px 16px;
  background-color: var(--background);
  box-shadow: 12px 0 12px var(--secondary);
  z-index: 10;
  letter-spacing: 1.6px;
}
</style>
