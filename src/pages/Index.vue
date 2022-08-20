<template>
  <Layout>
    <div class="container">
      <section class="section top">
        <h1 class="top__greeting">Hi, I'm Michał Wieczorek</h1>
        <Paragraph>
          This site is a presentation of myself as a web developer. I have been seriously
          investing in my skills since {{ learningTime }}.
        </Paragraph>
      </section>
      <section class="section middle">
        <Quote
          class="middle__item"
          author="myself"
          text="I learn from online courses, tutorials, books and practice. I love web and want to be part of its future. I want to build apps that will provide real value to users"
        />
        <Paragraph class="middle__item">
          <span>On this site you will find:</span>
          <Link class="middle__link" to="/skills"> skills and tools I have learned </Link>
          <Link class="middle__link" to="/portfolio"> Projects I have made </Link>
        </Paragraph>
      </section>
      <section class="section bottom">
        <Paragraph class="bottom__item">
          <Link to="/contact"> Contact me </Link>
        </Paragraph>
      </section>
    </div>
  </Layout>
</template>

<script>
import Paragraph from "../components/Paragraph.vue";
import Quote from "../components/Quote.vue";
import Link from "../components/Link.vue";
export default {
  metaInfo: {
    title: "Welcome!",
  },
  components: {
    Paragraph,
    Quote,
    Link,
  },
  computed: {
    learningTime: () => {
      const now = new Date();
      const startDate = new Date("01/01/2020");
      const years = now.getFullYear() - startDate.getFullYear();
      const months = now.getMonth() + 1;

      return `${years} years and ${months} months from now`;
    },
  },
};
</script>

<style scoped>
.container {
  margin: 0 20px;
  /* height: 100%; */

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}

.section {
  margin: 20px 0;
  max-width: 400px;
}

.top__greeting {
  margin-bottom: 10px;
  color: #fff;
  text-align: center;
}

.middle__item {
  margin: 20px 0;
}

.middle__link {
  margin-top: 20px;
}

.bottom {
  width: 100%;
  margin-top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.bottom__item {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (min-height: 500px) and (min-width: 800px), (min-height: 840px) {
  .container {
    height: 100%;
  }
}

@media (min-width: 800px) {
  .middle {
    max-width: 780px;
    display: flex;
    align-items: center;
    justify-content: space-around;
  }

  .middle__item {
    margin: 0 20px;
    width: 360px;
  }
}
</style>
