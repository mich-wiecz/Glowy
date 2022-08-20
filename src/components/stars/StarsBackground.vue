<template>
  <div class="page">
    <div
      v-if="show"
      @resize="adjustStars"
      ref="container"
      class="stars-bg"
      :style="{
        gridTemplateColumns: `repeat(${starsInRow}, 1fr)`,
        gridTemplateRows: `repeat(${starsInColumn}, 1fr)`,
      }"
    >
      <FallingStar
        v-for="(_, index) in Array(starsInColumn * starsInRow).fill()"
        :key="index"
        :width="starWidth"
      />
    </div>
    <slot />
  </div>
</template>

<script>
import FallingStar from "./FallingStar.vue";

export default {
  props: {
    show: {
      type: Boolean,
      default: true,
    },
  },
  components: {
    FallingStar,
  },
  data() {
    return {
      starWidth: 100,
      starsInRow: 1,
      starsInColumn: 1,
    };
  },
  methods: {
    adjustStars() {
      const {
        clientWidth: containerWidth,
        clientHeight: containerHeight,
      } = this.$refs.container;

      this.starsInRow = Math.floor(containerWidth / (this.starWidth * 2));
      this.starWidth = containerWidth / this.starsInRow / 2;

      const starHeight = (this.starWidth * Math.sqrt(3)) / 2;
      this.starsInColumn = Math.floor(containerHeight / (starHeight * 2));
    },
  },
  mounted() {
    this.adjustStars();
  },
};
</script>

<style>
.page {
  min-height: 100vh;
  position: relative;
}

.stars-bg {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  overflow: hidden;
  display: grid;
  justify-items: center;
  align-items: center;
}
</style>
