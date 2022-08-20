<template>
  <div class="quote">
    <g-image class="quote__icon" src="../assets/left-quote.svg" width="20px" />
    <p class="quote__text">
      {{ text }}
    </p>
    <span class="quote__author"> ~ {{ author }} </span>
  </div>
</template>

<script>
export default {
  props: {
    text: String,
    author: String,
  },
};
</script>

<style>
.quote {
  position: relative;
  padding: 20px;
  border-left: 10px solid var(--secondary);
  background-color: black;
  color: rgb(207, 204, 204);
  border-radius: 10px;

  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
}

.quote__icon {
  position: absolute;
  top: 10px;
  left: 10px;
}

.quote__text {
  margin-top: 20px;
  font-style: italic;
}

.quote__author {
  margin-top: 10px;
  align-self: flex-end;
}
</style>
