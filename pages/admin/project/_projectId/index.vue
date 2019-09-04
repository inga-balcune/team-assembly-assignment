<template>
  <div>
    <AppProjectForm :project="loadedProject" :clients='loadedClients' :team='loadedTeam' :sending='sending' @submit='onSubmitted'/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sending: false
    }
  },
  async asyncData({ $axios, params }) {
    const project = await $axios.$get(`https://signifly-assignment.firebaseio.com/projects/${params.projectId}.json`)
    const team = await $axios.$get(`https://signifly-assignment.firebaseio.com/team.json`)
    let teamArray = []
      for (const key in team) {
        teamArray.push({...team[key], id: key})
      }
    const client = await $axios.$get(`https://signifly-assignment.firebaseio.com/client.json`)
    let clientArray = []
      for (const key in client) {
        clientArray.push({...client[key], id: key})
      }
    return {loadedProject: project, loadedTeam: teamArray, loadedClients: clientArray }
  },
  methods: {
    onSubmitted(loadedProject) {
      this.sending = true
      return this.$axios.$put(`https://signifly-assignment.firebaseio.com/projects/${this.$route.params.projectId}.json`, loadedProject)
      .then(() => {
        this.sending = false
        this.$router.replace('/admin')
      })
      .catch(e => this.$router.replace('/error'))
    }
  }
}
</script>

<style lang="scss">
  
</style>