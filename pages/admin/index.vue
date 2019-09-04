<template>
  <div>
    <md-tabs md-alignment="centered" class='admin-section'>
      <md-tab id="tab-home" md-label="Dashboard">
        <section class="md-layout md-gutter md-alignment-center dashboard-card-warapper">
          <AppDashboardCard title='Team Members' :count='team.length' icon='assignment_ind'
                            class="md-layout-item md-xlarge-size-25 md-large-size-33 md-medium-size-33 md-small-size-50 md-xsmall-size-100"/>
          <AppDashboardCard title='Clients' :count='clients.length' icon='group'
                            class="md-layout-item md-xlarge-size-25 md-large-size-33 md-medium-size-33 md-small-size-50 md-xsmall-size-100"/>
          <AppDashboardCard title='Projects' :count='projects.length' icon='assignment'
                            class="md-layout-item md-xlarge-size-25 md-large-size-33 md-medium-size-33 md-small-size-50 md-xsmall-size-100"/>
        </section>
      </md-tab>
      <md-tab id="tab-pages" md-label="Team">
        <md-button class='md-fab add-new-button' title='Add new employee' to='/admin/member/new-team-member'>
          <md-icon>add</md-icon>
        </md-button>
        <AppEmployeeList :team='team'/>
      </md-tab>
      <md-tab id="tab-posts" md-label="Clients">
        <md-button class='md-fab add-new-button' title='Add a client' to='/admin/client/new-client'>
          <md-icon>add</md-icon>
        </md-button>
        <AppClientList :clients='clients'/>
      </md-tab>
      <md-tab id="tab-favorites" md-label="Projects">
        <md-button class='md-fab add-new-button' title='Create project' to='/admin/project/new-project'>
          <md-icon>add</md-icon>
        </md-button>
        <AppProjectList :projects='projects' :clients='clients'/>
      </md-tab>
    </md-tabs>
  </div>
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

.admin-section {
  margin-top: 2rem;
}

.dashboard-card-warapper {
  margin-top: 4rem;
}

.add-new-button {
  display: block;
  text-align: center;
  margin: 1rem auto 2rem auto;
}


  
</style>