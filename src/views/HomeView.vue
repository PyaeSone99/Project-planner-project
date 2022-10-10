<template>
  <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="current = $event" :current="current"></FilterNav>
    <div class="" v-for="project in filterProject" :key="project.id">
      <SingleProject :project="project" @delete="deleteProject" @complete="completeProject"></SingleProject>
    </div>
  </div>
</template>

<script>

import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'
export default {
  name: 'HomeView',
  components: {
    FilterNav,
    SingleProject,

  },
  data(){
    return{
      projects : [],
      current : "all"
    }
  },
  methods:{
    deleteProject(id){
      this.projects = this.projects.filter(projects => projects.id != id)
    },
    completeProject(id){
      let completeProject = this.projects.find(projects =>{
        return projects.id === id
      })
      completeProject.complete =! completeProject.complete
    }
  },
  mounted(){
    fetch('http://localhost:3000/projects')
    .then((res)=>{
      return res.json()
    })
    .then((data)=>{
      this.projects = data
    })
    .catch(()=>{

    })
  },
  computed:{
    filterProject(){
      if(this.current === "complete"){
        return this.projects.filter(ele => ele.complete)
      }
      if(this.current === 'ongoing'){
        return this.projects.filter(ele => !ele.complete)
      }
      return this.projects
    }
  }
}
</script>
