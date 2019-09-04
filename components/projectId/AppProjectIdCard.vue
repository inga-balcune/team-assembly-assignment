<template>
  <div class="card-expansion">

    <md-card md-with-hover>
      <md-ripple>
        <md-card-header>
          <div class="md-title">{{title}}</div>
          <div class="md-subhead">{{clientName}}</div>
        </md-card-header> 
  

        <md-card-actions>
          <md-button :to="`/projects/${title.replace(/ +/g, '-').toLowerCase()}`">View Project</md-button>
        </md-card-actions>
      </md-ripple>
    </md-card>
  </div>
</template>

<script>
export default {
  props: {
      title: {
        type: String,
        required: true
      },
      client: {
        type: String,
        required: true
      },
      id: {
        type: String,
        required: true
      },
      clients: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        clientName: null
      }
    },
    methods: {
      deleteProject() {
        if (window.confirm("Do you really want to delete?")) { 
          return this.$axios.$delete(`https://signifly-assignment.firebaseio.com/projects/${this.id}.json`, { key: this.id })
                .then(response => {
                  this.$router.go(0)
                })
        }
      },
      projectLink() {
        return `admin/project/${this.id}`
      },
      copyURL() {
        let copyURL = document.getElementById(`${this.id}-projectURL`);
        copyURL.select()
        copyURL.setSelectionRange(0, 99999)
        document.execCommand("copy");
        alert("Copied the text: " + copyURL.value);
      },
      getClientName() {
        const newClient = this.clients.filter(client => client.id.includes(this.client))
        this.clientName = newClient[0].client
        
        console.log(this.clientName)
        // this.clientName = newClient[0].client
      }
    },
    mounted() {
      this.getClientName()
    }
  
}
</script>

<style lang="scss" scoped>

.projectURL {
  opacity: 0;
}

.md-card-header {
  margin-bottom: 2rem;
}
</style>