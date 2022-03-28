<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '@/components/FilterNav';
export default {
  name: 'HomeView',
  components: {
    SingleProject,
    FilterNav
  },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },

  computed: {
    filteredProjects() {
      if(this.current === 'completed') {
        return this.projects.filter(project => project.complete);
      }
      else if(this.current === 'ongoing') {
        return this.projects.filter(project => !project.complete);
      }
      return this.projects;
    }
  },

  mounted() {
     fetch('http://localhost:3001/projects')
        .then(res => res.json())
        .then(data => this.projects = data)
        .catch(error => console.log(err.message));
  },

  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter(project => {
        return project.id != id
      });
    },

    handleComplete(id) {
      let p = this.projects.find(project => { return project.id === id })
      p.complete = !p.complete;
      console.log(p);
    }
  }

  
}
</script>
