<template>
  <div class="switch">
    <div
      @click="$emit('nightsky')"
      class="switch__item nightsky"
      :class="{ active: nightSky }"
    />
    <div @click="$emit('stars')">
      <Star
        class="switch__item stars"
        :class="{ active: stars }"
        :width="50"
        :branches="1"
        :animated="false"
      />
    </div>
  </div>
</template>

<script>
import Star from "./stars/Star.vue";

export default {
  name: "MotivesSwitch",
  components: {
    Star,
  },
  props: {
    nightSky: Boolean,
    stars: Boolean,
  },
};
</script>

<style scoped>
.switch {
  display: flex;
  align-items: center;
}

.switch__item {
  width: 40px;
  height: 40px;
  border: 2px solid var(--darker);
  cursor: pointer;
}

.switch__item.active {
  border-color: var(--primary);
}

.switch__item.nightsky {
  background: repeat url("../assets/darkness.png");
  border-radius: 20px 0 0 20px;
}

.switch__item.stars {
  margin-left: 8px;
  border-radius: 0 20px 20px 0;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
