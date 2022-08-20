<template>
  <nav class="nav">
    <div class="nav__limiter">
      <g-link to="/">
        <Logo class="nav__logo" />
      </g-link>
      <div class="links">
        <g-link
          class="links__item"
          v-for="(item, index) in links"
          :key="index"
          :to="item.link"
        >
          <p class="links__item-text">
            {{ item.title }}
          </p>
          <g-image
            :src="require(`!!assets-loader!@images/${item.iconURL}`)"
            class="links__icon"
            :class="item.class"
          />
        </g-link>
      </div>
      <MotiveSwitch
        class="motive-switch"
        :stars="stars"
        :nightSky="nightSky"
        @stars="$emit('stars')"
        @nightsky="$emit('nightsky')"
      />
    </div>
  </nav>
</template>

<script>
import Logo from "./Logo.vue";
import MotiveSwitch from "./MotiveSwitch.vue";

export default {
  name: "Nav",
  components: {
    Logo,
    MotiveSwitch,
  },
  props: {
    stars: Boolean,
    nightSky: Boolean,
  },
  data() {
    return {
      links: [
        {
          title: "Skills",
          link: "/skills",
          iconURL: "bookshelf.svg",
          class: "bookshelf-icon",
        },
        {
          title: "Portfolio",
          link: "/portfolio",
          iconURL: "briefcase.svg",
        },
        {
          title: "Contact",
          link: "/contact",
          iconURL: "socket.svg",
        },
      ],
    };
  },
};
</script>

<style scoped>
.nav {
  background-color: var(--background);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav__limiter {
  padding: 4px 12px;
  width: 100%;
  height: 100%;
  max-width: 1000px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.links {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}

.links__item {
  display: block;
  height: 50px;
  margin-left: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: 0.3s ease-in-out;
  cursor: pointer;
}

.links__item:hover {
  transform: scale(0.92);
}

.links__item-text {
  display: none;
  padding: 4px 14px;
  width: 100px;
  height: 70%;
  border: 2px solid var(--primary);
  border-radius: 8px 0 0 8px;
  text-decoration: none;
  color: var(--primary);
  transform: translateX(15%);
  text-align: center;
}

.links__item:hover .links__item-text {
  box-shadow: -8px 5px 10px 1px var(--primary);
}

.links__icon {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: var(--primary);
  z-index: 11;
}

@media (min-width: 540px) {
  .nav__limiter {
    padding: 4px 24px;
  }

  .links__item {
    height: 50px;
    margin-left: 16px;
  }

  @media (min-width: 720px) {
    .links {
      flex-grow: 1;
    }

    .links__item-text {
      display: block;
    }
  }
}
</style>
