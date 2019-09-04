<template>
  <div>
    <md-card md-with-hover>
      <md-ripple>
        <md-card-header>
          <div class="md-title">{{title}}</div>
          <div class="md-subhead">{{clientName}}</div>
        </md-card-header>

        <md-card-content>
          {{description.length > 100 ? description.substring(0, 100) + '...' : description}}
        </md-card-content>

        <md-card-actions>
          <input class='projectURL' type="text" :value="setURL(title)" :id="`${id}-projectURL`">
          <md-button @click="copyURL()">Copy URL</md-button>
          <md-button :to='projectLink()'>Edit</md-button>
          <md-button @click='deleteProject'>Delete</md-button>
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
      pm: {
        type: String,
        required: true
      },
      team: {
        type: Array,
        required: true
      },
      id: {
        type: String,
        required: true
      },
      description: {
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
        windowLocation: null,
        clientName: null
      }
    },
    methods: {
      deleteProject() {
        if (window.confirm("Are you sure you want to delete this project?")) { 
          return this.$axios.$delete(`https://signifly-assignment.firebaseio.com/projects/${this.id}.json`, { key: this.id })
                .then(response => {
                  this.$router.go(0)
                })
        }
      },
      projectLink() {
        return `admin/project/${this.id}`
      },
      setURL(title) {
        return this.windowLocation + '/projects/' + title.replace(/ +/g, '-').toLowerCase()
      },
      copyURL() {
        let copyURL = document.getElementById(`${this.id}-projectURL`);
        copyURL.select()
        copyURL.setSelectionRange(0, 99999)
        document.execCommand("copy")
        alert("Copied the URL: " + copyURL.value);
      },
      getClientName() {
        const newClient = this.clients.filter(client => client.id.includes(this.client))
        this.clientName = newClient[0].client
      }
    },
     mounted() {
      this.windowLocation = window.location.origin
      this.getClientName()
    }
  
}
</script>

<style lang="scss" scoped>

.projectURL {
  // width: .0001%;
  opacity: 0;
}

.md-card {
  min-height: 30rem;
}

.md-card-header+.md-card-content {
  margin-bottom: 2rem;
}

</style>