<template>
  <div class="home">
    <FilterNav @filterChange="current=$event" :current="current"/>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
    <div v-else>
      <h3>There are no projects</h3>
    </div>
  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue';
import FilterNav from '@/components/FilterNav.vue';

export default {
  name: 'Home',
  components: { SingleProject,FilterNav },
  data() {
    return {
      projects: [],
      current:'all'
    };
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then((res) => res.json())
      .then((data) => (this.projects = data))
      .catch((err) => console.log(err.message));
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id;
      });
    },
    handleComplete(id) {
      const project = this.projects.find((project) => project.id === id);
      if (project) {
        project.complete = !project.complete;

        // Trigger reactivity after updating the project
        this.projects = [...this.projects];
      } else {
        console.error('Project not found:', id);
      }
    },
  },
  computed:{
    filteredProjects(){
      if(this.current==='completed'){
        return this.projects.filter(project=>project.complete)
      }
      if(this.current==='ongoing'){
        return this.projects.filter(project=>!project.complete)
      }
      return this.projects
    }
  }
};
</script>
