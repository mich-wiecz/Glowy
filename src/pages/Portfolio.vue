<template>
  <Layout>
    <div class="container">
      <ProjectItem
        class="project"
        v-for="project in projects"
        :key="project.title"
        v-bind="project"
      />
    </div>
  </Layout>
</template>

<script>
import projects from "../data/projects.json";
import ProjectItem from "../components/ProjectItem.vue";
export default {
  metaInfo: {
    title: "Portfolio",
  },
  components: {
    ProjectItem,
  },
  data() {
    return {
      projects,
    };
  },
};
</script>

<style scoped>
.container {
  margin: 20px 0px 0 0px;
  /* height: 100%; */

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}

.project {
  margin: 20px;
  max-width: 800px;
}
</style>
