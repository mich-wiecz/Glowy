<template>
  <div v-if="isExample" class="card card__example">
    <h6 class="card__title">&lt; Tool Name &gt;</h6>
    <div class="card__icon card__example-icon">&lt; Icon &gt;</div>
    <div class="card__rating">&lt; Subjective Rating &gt;</div>
  </div>
  <div v-else class="card">
    <h6 class="card__title">
      <slot />
    </h6>
    <g-image
      v-if="iconURL"
      class="card__icon"
      :src="require(`!!assets-loader!@images/tools/${iconURL}`)"
    />
    <Rating v-if="rating" class="card__rating" :value="rating" :type="rating.type" />
  </div>
</template>

<script>
import Rating from "./Rating.vue";
export default {
  components: {
    Rating,
  },
  props: {
    name: String,
    iconURL: String,
    rating: Number,
    isExample: Boolean,
  },
};
</script>

<style scoped>
.card {
  width: 150px;
  height: 150px;
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

.card__title {
  font-size: 16px;
}

.card__icon {
  margin-top: 10px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: var(--light);
}

.card__rating {
  margin-top: 10px;
}

.card__example {
  width: 195px;
  height: 195px;
}

.card__example-icon {
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--background);
}
</style>
