<template>
  <div>
    <div v-for='data in employee' :key='data.id'>
      <header>
        <h1 class='employee-title'>
          {{data.firstName}} {{data.lastName}}
        </h1>
        <h4 class='employee-designation'>
          {{data.designation}}
        </h4>
      </header>

      <main>
        <div class="md-layout md-gutter project-meta-container">
          <section class="md-layout-item md-xlarge-size-30 md-large-size-30 md-medium-size-30 md-small-size-45 md-xsmall-size-100 project-meta">
            <md-card-media md-ratio="1:1">
              <img :src="data.image" :alt="`${data.firstName} ${data.lastName} image`" :title="`Photo of ${data.firstName} ${data.lastName}`">
            </md-card-media>
          </section>
          <section class="md-layout-item md-xlarge-size-70 md-large-size-70 md-medium-size-70 md-small-size-55 md-xsmall-size-100">
            <div class='project-description'>
              {{data.background}}
            </div>
            <div class='project-contact-info'>
               <p><span class='project-contact-info--item'>Email: </span><a :href="`mailto:${data.email}`">{{data.email}}</a></p>
               <p><span class='project-contact-info--item'>Tel: </span><a :href="`tel:+45${data.number}`">+45 {{data.number.length === 8 ? data.number.match(/.{1,4}/g).join(' ') : data.number}}</a></p>
            </div>
          </section>
        </div>
        <div v-if='pmInProjects != ""'>
          <h2>Currently Managing Projects</h2>
          <AppProjectIdList :projects='pmInProjects' :clients='clients'/>
        </div>

        <div v-if='otherProjects.length > 0'>
          <h2>{{pmInProjects != "" ? 'Other Projects' : 'Projects'}}</h2>
          <AppProjectIdList :projects='otherProjects' :clients='clients'/>
        </div>
      </main>
    </div>
    
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      employee: null,
      employeeId: null,
      pmInProjects: "",
      otherProjects: []
    }
  },
  created() {
    this.findEmployee()
    this.findProjects()
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
    findEmployee() {
      const nameArray = this.$route.params.id.split("-")
      const name = nameArray.shift()
      const surname = nameArray.pop()
     
    // find employee 
      this.employee = this.team.filter(member => member.firstName.toLowerCase().includes(name) && member.lastName.toLowerCase().includes(surname))

    // get employee id
      let employeeId = this.employee.map(property => property.id)
      this.employeeId = employeeId.join()
    },
    findProjects() {
      this.pmInProjects = this.projects.filter(project => project.projectManager.includes(this.employeeId))

      // let projectTeam = this.projects.filter(project => project.selectedTeam.filter(member => member.includes(this.employeeId)))
      this.otherProjects = this.projects.filter(project => project.selectedTeam.includes(this.employeeId))
      // console.log(projectTeam)
      
 
    }
  }
}
</script>

<style lang="scss" scoped>

.employee-title {
  margin-bottom: 0.5rem;
}

.employee-designation {
  margin-top: 0;
  font-weight: 700;
}

.project-description {
  @include respond (phone) {
    margin-top: 2rem;
  }
}

.project-contact-info {
  margin-top: 2rem;

  &--item {
    font-weight: 700;
  }
}

.md-card-media.md-ratio-1-1:before {
    padding-top: 69%;
  }


.md-card-media.md-ratio-1-1 img {
  top: 90%;
}
  
</style>