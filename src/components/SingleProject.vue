<template>
    <div class="project" :class="{complete: project.complete}">
      <div class="actions">
        <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
        <div class="icons">
          <router-link :to="{name:'EditProject', params:{id:project.id}}">
            <span class="material-icons">edit</span>
          </router-link>
          <span class="material-icons" @click="toggleComplete">done</span>
          <span class="material-icons" @click="deleteProject">delete</span>
          
        </div>
      </div>
      <div v-if="showDetails" class="details">
        {{ project.details }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['project'],
    data() {
      return {
        showDetails: false,
        url: `http://localhost:3000/projects/${this.project.id}`,
      };
    },
    methods: {
      deleteProject() {
        fetch(this.url, { method: 'DELETE' })
          .then(() => {
            this.$emit('delete', this.project.id);
            console.log('Project deleted successfully');
          })
          .catch((err) => console.log(err.message));
      },
      toggleComplete() {
        fetch(this.url, {
          method: 'PATCH',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ complete: !this.project.complete }),
        })
          .then(() => {
            console.log('Toggle complete');
            this.$emit('complete', this.project.id);
          })
          .catch((err) => console.log(err.message));
      },
    },
  };
  </script>
  
  <style>
  .project {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid #e90074;
  }
  h3 {
    cursor: pointer;
  }
  .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .material-icons:hover {
    color: #777;
  }
  .project.complete {
    border-left: 4px solid #00ce89;
  }
  </style>
  