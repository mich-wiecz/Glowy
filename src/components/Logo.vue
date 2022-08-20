<template>
  <div class="outer">
    <div class="inner" />
  </div>
</template>

<script>
export default {};
</script>

<style>
.outer {
  position: relative;
  width: 35px;
  height: 35px;
  border-radius: 50px;
  background-color: transparent;
  border: 10px solid transparent;
  box-shadow: 0 0 15px 7px var(--primary);
}

.inner {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 0.01px;
  height: 0.01px;
  border-radius: 50px;
  background-color: transparent;
  box-shadow: 0 0 25px 20px var(--primary);
}
</style>
