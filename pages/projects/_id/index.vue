<template>
  <div>
    <div v-for='item in project' :key='item.id' class='project-container'>
      <header>
        <h1 class='project-title'>
          {{item.title}}
        </h1>
      </header>
      <main>
        <div class="md-layout md-gutter project-meta-container">
          <section class="md-layout-item md-xlarge-size-70 md-large-size-60 md-medium-size-60 md-small-size-60 md-xsmall-size-100 project-description">{{item.description}}</section>
          <section class="md-layout-item md-xlarge-size-30 md-large-size-40 md-medium-size-40 md-small-size-40 md-xsmall-size-100 project-meta">
            <div v-for='data in client' :key='data.id'>
              <div class='meta-item'>
                <div class='item-label'>Client:</div>
                <div>{{data.client}}</div>
              </div>
              <div class='meta-item'>
                <div class='item-label'>Project Lead:</div>
                <div>{{data.firstName}} {{data.lastName}}</div>
              </div>
              <div class='meta-item'>
                <div class='item-label'>Email:</div>
                <div><a :href="`mailto:${data.email}`">{{data.email}}</a></div>
              </div>
              <div class='meta-item'>
                <div class='item-label'>Phone:</div>
                <div><a :href="`tel:+45${data.number}`">+45 {{data.number.length === 8 ? data.number.match(/.{1,4}/g).join(' ') : data.number}}</a></div>
              </div>
            </div>
          </section>
        </div>
        <div class="md-layout md-gutter">
          <div class="md-layout-item md-xlarge-size-20 md-large-size-25 md-medium-size-25 md-small-size-33 md-xsmall-size-100">
              <h2>Project Manager</h2>
              <AppEmployeeIdList :team='projectManager'/>
          </div>
        
          <div class="md-layout-item md-xlarge-size-80 md-large-size-75 md-medium-size-75 md-small-size-66 md-xsmall-size-100">
            <h2>Project Team</h2>
            <AppEmployeeIdList :team='projectTeam'/>
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      project: null,
      projectManager: null,
      projectTeam: null,
      client: null
    }
  },
  created() {
    this.findProject()
    this.findProjectManager()
    this.findClient()
    this.findProjectTeam()
  },
  async asyncData({ $axios, params, error }) {
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
  },
  methods: {
    findProject() {
      this.project = this.projects.filter(project => project.title.replace(/ +/g, '-').toLowerCase().includes(this.$route.params.id))
    },
    findProjectManager() {
      let pm = this.project.map(data => data.projectManager).join()
      this.projectManager = this.team.filter(member => member.id.includes(pm))
    },
    findClient() {
      let projectClient = this.project.map(data => data.client).join()
      this.client = this.clients.filter(client => client.id.includes(projectClient))
    },
    findProjectTeam() {
      let projectTeam = this.project.map(data => data.selectedTeam)
      this.projectTeam = this.team.filter(member => projectTeam[0].includes(member.id))
    }
  },
}
</script>

<style lang="scss" scoped>

.project-container {
  min-height: 100vh;
}

.project-title {
  text-align: center;
}

.project-description {
  padding: 2rem;
}

.project-meta {
  padding: 2rem;
  
}

.meta-item {
  width: 100%;
  display: flex;
  flex-direction: row;
}

.item-label {
  font-weight: 500;
  margin-right: 1rem;
}
  
</style>