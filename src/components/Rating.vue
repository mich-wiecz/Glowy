<template>
  <div v-if="type === 'progress'" class="progress">
    <div class="progress__bar" />
    <div :style="{ left: `${(value / max) * 100}%` }" class="progress__hider" />
  </div>
  <div v-else class="stars">
    <g-image
      class="stars__item"
      v-for="star in Array(value).fill()"
      :key="star"
      src="@images/star.svg"
      width="15"
    />
  </div>
</template>

<script>
export default {
  props: {
    value: [Number, String],
    max: Number,
    type: String,
  },
};
</script>

<style>
.stars {
  padding: 5px 10px;
  display: flex;
  justify-content: space-evenly;
}

.stars__item {
  margin: 0 2px;
}

.progress {
  position: relative;
  padding: 4px;
  width: 100%;
  height: 16px;
  background-color: black;
  box-shadow: 0 0 4px 4px var(--secondary);
  border-radius: 10px;
}

.progress__bar {
  width: 100%;
  height: 100%;
  background-image: linear-gradient(to right, green, yellow 50%, red);
  border-radius: 10px;
}

.progress__hider {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  background-color: black;
  border-radius: 0 10px 10px 0;
}
</style>
