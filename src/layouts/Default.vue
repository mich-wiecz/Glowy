<template>
  <StarsBackground :show="stars" class="site" :class="{ nightSky }">
    <Nav
      class="site__nav"
      :stars="stars"
      :nightSky="nightSky"
      @stars="stars = !stars"
      @nightsky="nightSky = !nightSky"
    />
    <main class="site__main">
      <slot />
    </main>
    <Footer class="site__footer" />
  </StarsBackground>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
import StarsBackground from "../components/stars/StarsBackground.vue";
import Nav from "../components/Nav.vue";
import Footer from "../components/Footer.vue";

export default {
  components: {
    StarsBackground,
    Nav,
    Footer,
  },
  data() {
    return {
      nightSky: false,
      stars: true,
    };
  },
};
</script>

<style>
:root {
  --primary: rgb(87, 142, 206);
  --secondary: rgb(26, 96, 177);
  --dark: rgb(36, 89, 150);
  --darker: rgb(9, 44, 85);
  --background: #000;
  --light: #fff;
}

body {
  min-height: 100vh;
  font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", Roboto,
    "Helvetica Neue", Arial, sans-serif;
  margin: 0;
  padding: 0;
  line-height: 1.5;
  background-color: black;
  color: var(--primary);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.site {
  height: 100vh;
  width: 100vw;
  overflow: hidden;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  z-index: 1;
}

.nightSky {
  background: repeat url("../assets/darkness.png");
}

.site__nav {
  width: 100vw;
  height: 60px;
  box-shadow: 2px 0 9px var(--secondary);
}

.site__main {
  flex-grow: 1;
  width: 100vw;
  overflow-y: scroll;
  overflow-x: hidden;
  z-index: 2;
}

.site__footer {
  width: 100vw;
}
</style>
