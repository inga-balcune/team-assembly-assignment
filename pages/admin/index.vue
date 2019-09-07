<template>
  <main>
    <AppDashboardTabs :team='team' :clients='clients' :projects='projects' />
  </main>
</template>

<script>
import axios from 'axios'
export default {
  layout: 'admin',
  async asyncData({ $axios, params }) {
    const employees = await $axios.$get(`https://signifly-assignment.firebaseio.com/team.json`)
      let teamArray = []
      for (const key in employees) {
        teamArray.push({...employees[key], id: key})
      }
    const clients = await $axios.$get(`https://signifly-assignment.firebaseio.com/client.json`)
      let clientArray = []
      for (const key in clients) {
        clientArray.push({...clients[key], id: key})
      }

    const projects = await $axios.$get(`https://signifly-assignment.firebaseio.com/projects.json`)
      let projectsArray = []
      for (const key in projects) {
        projectsArray.push({...projects[key], id: key})
      }
    return { team: teamArray, clients: clientArray, projects: projectsArray}
  }
}
</script>

<style lang="scss" scoped>


  
</style>