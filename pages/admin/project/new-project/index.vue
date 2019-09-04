<template>
  <div>
    <AppProjectForm @submit='onSubmitted' :sending='sending' :team='team' :clients='clients'/>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      sending: false
    }
  },
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
      
    return { team: teamArray, clients: clientArray }
  },
  methods: {
    onSubmitted(projectData) {
    this.sending = true
    return axios.post(`https://signifly-assignment.firebaseio.com/projects.json`, projectData)
    .then(() => {
      this.sending = false
      this.$router.push(`/projects/${projectData.title.replace(/ +/g, '-').toLowerCase()}`)
    })
   }
  }
 
}
</script>

<style lang="scss">
  
</style>