<template>
  <ul class="list">
    <li class="list__item" v-for="(text, index) in items" :key="index">
      {{ text }}
    </li>
  </ul>
</template>

<script>
export default {
  name: "List",
  props: {
    items: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style scoped>
.list {
  list-style: none;
}

.list__item {
  position: relative;
  margin: 10px 0;
  margin-left: 40px;
}

.list__item::before {
  display: block;
  content: "";
  position: absolute;
  top: 50%;
  bottom: 0;
  left: -40px;
  width: 20px;
  background-color: var(--primary);
  transform: translateY(-50%);
}
</style>
