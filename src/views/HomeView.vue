<template>
  <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="current=$event" :current="current"></FilterNav>
    <div v-for="project in filteredProjects" :key="project.id"> 
      <SingleProject :project="project" @delete="deleteProject" @complete="completeProject"  />
    </div>
  </div>
  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">
</template>
<script>
import SingleProject from "../components/SingleProject";
import FilterNav from "../components/FilterNav";

// @ is an alias to /src
export default {
  name: "HomeView",
  components: {
    SingleProject,
    FilterNav,
  },
  data() {
    return {
      projects: [],
      current: "all"
    };
  },
  computed: {
    filteredProjects() {
      if (this.current === 'complete') {
        return this.projects.filter((project) => {
          return project.complete;
        });
      }
      if (this.current === 'ongoing') {
        return this.projects.filter((project) => {
          return !project.complete;
        });
      }
      return this.projects;
    }
  },
  mounted() {
    fetch("http://localhost:3000/projects")
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.projects = data;
      })
      .catch((err) => {
        console.log(err);
      });
  },
  methods: {
    deleteProject(id) {
      this.projects = this.projects.filter(project => {
        return project.id != id;
      });
    },
    completeProject(id) {
      let findProject = this.projects.find((project) => {
        return id === project.id;
      });
      findProject.complete = !findProject.complete;
    },
  },
};
</script>
