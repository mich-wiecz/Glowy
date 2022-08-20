<template>
  <Layout>
    <div class="container">
      <div class="top">
        <section class="section education">
          <Title class="title">Education</Title>
          <SkillCard class="education__description">
            I have high school education. At the moment I am not studying at the
            university.
          </SkillCard>
        </section>
        <section class="section languages">
          <Title class="title">Languages</Title>
          <LanguageCard
            class="languages__item"
            name="Polish"
            iconURL="pol-flag.svg"
            level="Mother tongue"
          />
          <LanguageCard
            class="languages__item"
            name="English"
            iconURL="gb-flag.svg"
            description="I understand both speech and writing (including documentation) without any problem. I have less experience with language production."
            level="B2 / C1"
          />
        </section>
      </div>

      <section class="section tools">
        <header class="tools__header">
          <Title class="title">Toolkit</Title>
          <SkillCard isExample />
        </header>

        <div class="tools__section" v-for="(section, index) in tools" :key="index">
          <SkillCard
            class="tools__item"
            v-for="{ name, iconURL, rating } in section"
            :key="name"
            :iconURL="iconURL"
            :rating="rating"
            >{{ name }}</SkillCard
          >
        </div>
      </section>
    </div>
  </Layout>
</template>

<script>
import Title from "../components/Title.vue";
import SkillCard from "../components/SkillCard.vue";
import LanguageCard from "../components/LanguageCard.vue";
import tools from "../data/tools.json";
export default {
  metaInfo: {
    title: "Tools",
  },
  components: {
    Title,
    SkillCard,
    LanguageCard,
  },
  computed: {
    tools() {
      return tools.map((toolsSection) => {
        return toolsSection.sort((a, b) => {
          if (a.rating > b.rating) {
            return -1;
          }
          if (b.rating > a.rating) {
            return 1;
          }
          return 0;
        });
      });
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
  width: 100%;
  max-width: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.title {
  margin: 0 auto 30px auto;
  text-align: center;
}

.top {
  margin-bottom: 20px;
}

.education__description {
  width: 100%;
}

.languages__item {
  width: 100%;
  margin-bottom: 10px;
}

.section.tools {
  max-width: 490px;
  align-items: flex-start;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.tools__section {
  width: 320px;
  margin-bottom: 40px;
  display: inline-flex;
  flex-wrap: wrap;
  /* justify-content: center; */
}

.tools__header {
  margin-bottom: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.tools__item {
  margin: 5px;
}

@media (min-width: 520px) {
  .tools__section {
    width: 480px;
  }
}

@media (min-width: 640px) {
  .tools__header {
    width: 100%;
    max-width: 640px;
    flex-direction: row;
    align-items: flex-start;
    justify-content: space-between;
  }
}

@media (min-width: 720px) {
  .tools__section {
    width: 640px;
  }

  .section.tools {
    max-width: 640px;
  }
}

@media (min-width: 830px) {
  .top {
    width: 100%;
    display: flex;
    justify-content: space-evenly;
  }
  .tools__section {
    width: 800px;
  }
  .section.tools {
    max-width: 810px;
  }
}
</style>
