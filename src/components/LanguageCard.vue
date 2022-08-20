<template>
  <div class="card">
    <header class="card__header header">
      <g-image
        v-if="iconURL"
        class="header__icon"
        :src="require(`!!assets-loader!@images/${iconURL}`)"
      />
      <h6 class="header__title">
        {{ name }}
      </h6>
    </header>
    <p class="card__description" v-if="description">
      {{ description }}
    </p>
    <p class="card__level" v-if="level">Level: {{ level }}</p>
  </div>
</template>

<script>
export default {
  name: "LanguageCard",
  props: {
    name: String,
    iconURL: String,
    description: String,
    level: String,
  },
};
</script>

<style scoped>
.card {
  padding: 10px 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: var(--dark);
  color: var(--light);
  border: 1px solid var(--secondary);
  border-radius: 10px;
  box-shadow: 0 0 8px 4px var(--secondary);
  cursor: crosshair;
}

.card:hover {
  background-color: var(--darker);
}

.header {
  width: 100%;
  display: flex;
}

.header__icon {
  width: 40px;
  height: 30px;
}

.header__title {
  flex-grow: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 16px;
}

.card__description {
  margin-top: 10px;
}

.card__level {
  margin-top: 10px;
}
</style>
