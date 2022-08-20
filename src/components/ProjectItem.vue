<template>
  <div class="project">
    <section class="project__header">
      <div class="header__title-container">
        <h5 class="project__title">
          {{ title }}
        </h5>
      </div>
      <div class="project__image-container">
        <g-image
          class="project__image"
          :src="require(`!!assets-loader!@images/projects/${imageURL}`)"
        />
        <a
          role="button"
          :href="projectLink"
          target="_blank"
          rel="noopener noreferrer"
          class="project__project-link"
        >
          Visit
        </a>
      </div>
      <div v-if="guide" class="project__guide-attribution">
        <p class="project__guide-note">Project created by following a tutorial</p>
        <div class="project__guide">
          <span>Guide: </span>
          <span>{{ guide }}</span>
        </div>
        <div v-if="tutorialLink" class="project__link-container">
          <span>Tutorial: </span>
          <a
            class="project__link"
            :href="tutorialLink"
            target="_blank"
            rel="noopener noreferrer"
          >
            {{ tutorialLink }}
          </a>
        </div>
      </div>
      <div class="project__link-container">
        <span>GitHub: </span>
        <a
          class="project__link"
          :href="githubLink"
          target="_blank"
          rel="noopener noreferrer"
        >
          {{ githubLink }}
        </a>
      </div>
    </section>
    <section class="project__main">
      <div class="project__features">
        <h4>Features:</h4>
        <List v-if="features.length > 0" :items="features" />
      </div>

      <div class="project__tools-container">
        <p class="project__tools-label">Created with the use of:</p>
        <div class="project__tools">
          <div class="project__tool tool" v-for="(tool, index) in tools" :key="index">
            <g-image
              v-if="tool.iconURL"
              :src="require(`!!assets-loader!@images/tools/${tool.iconURL}`)"
              class="tool__icon"
            />
            <span class="tool__title">{{ tool.name }}</span>
          </div>
        </div>
      </div>
      <p v-if="!completed" class="project__in-progress">
        Work on this project is still in progress
      </p>
    </section>
  </div>
</template>

<script>
import List from "./List.vue";
export default {
  name: "ProjectItem",
  components: {
    List,
  },
  props: {
    title: String,
    guide: String,
    tutorialLink: String,
    githubLink: String,
    imageURL: String,
    projectLink: String,
    features: {
      type: Array,
      default() {
        return [];
      },
    },
    completed: {
      type: Boolean,
      default: true,
    },
    tools: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style scoped>
.project {
  padding: 10px 20px;
  background-color: var(--dark);
  color: var(--light);
  border-radius: 10px;
  cursor: crosshair;
}

.project:hover {
  background-color: var(--darker);
}

.project__header {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: var(--light);
}

.header__title-container {
  width: 90%;
  padding: 0 20px;
}

.project__title {
  width: 100%;
  text-align: center;
  font-size: 2rem;
  font-weight: normal;
}

.project__guide-attribution {
  width: 90%;
  margin-top: 30px;
  color: var(--light);
  margin-bottom: 20px;
}

.project__guide-note {
  margin-bottom: 10px;
}

.project__link-container {
  width: 90%;
  overflow: hidden;
}

.project__link {
  display: inline;
  color: var(--primary);
}

.project__image-container {
  position: relative;
  padding: 10px;
  flex-grow: 1;
}

.project__image {
  width: 100%;
  border-radius: 5px;
}

.project__project-link {
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  text-align: center;
  padding: 8px 32px;
  background-color: var(--darker);
  border-radius: 8px;
  box-shadow: 0 0 5px 5px var(--secondary);
  cursor: pointer;
  text-transform: uppercase;
  color: var(--light);
  text-decoration: none;
  letter-spacing: 2px;
}

.project__features {
  margin: 30px auto;
  width: 90%;
}

.project__tools-container {
  margin: 30px auto;
  width: 90%;
}

.project__tools-label {
  margin-bottom: 10px;
}

.project__tools {
  display: flex;
  flex-wrap: wrap;
}

.tool {
  margin-bottom: 10px;
  margin-right: 10px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  padding: 5px 10px;
  background-color: rgb(43, 22, 163);
  border-radius: 10px;
}

.tool__icon {
  margin-right: 10px;
  padding: 2px;
  width: 30px;
  height: 30px;
  background-color: #fff;
  border-radius: 50%;
}

.project__in-progress {
  text-align: center;
}
</style>
