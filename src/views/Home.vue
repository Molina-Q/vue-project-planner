<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <!-- current will store the value given from the the filterNav -->
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject
          :project="project"
          @delete="handleDelete"
          @complete="handleComplete"
        />
      </div>
    </div>
  </div>
</template>

<script>
import FilterNav from "@/components/FilterNav.vue";
import SingleProject from "@/components/SingleProject.vue";

export default {
  name: "Home",
  data() {
    return {
      projects: [],
      current: "all",
    };
  },
  components: {
    SingleProject,
    FilterNav,
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    handleComplete(id) {
      let p = this.projects.find((project) => project.id === id);
      p.complete = !p.complete;
    },
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((response) => response.json())
      .then((data) => (this.projects = data))
      .catch((error) => console.error(error));
  },
  computed: {
    filteredProjects() {
      if (this.current === "ongoing") {
        return this.projects.filter((project) => !project.complete);
      } else if (this.current === "completed") {
        return this.projects.filter((project) => project.complete);
      } else {
        return this.projects;
      }
    },
  },
};
</script>
