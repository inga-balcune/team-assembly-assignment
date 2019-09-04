<template>
  <div>
    <md-card>
      <md-card-header>
        <md-card-header-text>
          <div class="md-title">{{client}}</div>
          <div class="md-subhead">{{name}} {{surname}}</div>
        </md-card-header-text>

        <md-card-media v-if='logo'>
          <img :src="logo" :alt="`${client} logo`">
        </md-card-media>
      </md-card-header>

      <md-card-actions>
        <md-button @click='deleteClient'>Delete</md-button>
        <md-button :to='clientLink()'>Edit</md-button>
      </md-card-actions>
    </md-card>
  </div>
</template>

<script>
export default {
  props: {
      name: {
        type: String,
        required: true
      },
      surname: {
        type: String,
        required: true
      },
      client: {
        type: String,
        required: true
      },
      logo: {
        // type: String,
        // required: true
      },
      id: {
        type: String,
        required: true
      }
    },
  methods: {
    deleteClient() {
      if (window.confirm("Are you sure you want to delete this client?")) { 
        return this.$axios.$delete(`https://signifly-assignment.firebaseio.com/client/${this.id}.json`, { key: this.id })
        .then(response => {
          this.$router.go(0)
          })
      }    
    },
    clientLink() {
      return `admin/client/${this.id}`
    }
  }
}
</script>

<style lang="scss">

  .md-card {
    min-height: 20rem;
  }

  .md-card-header.md-card-header-flex {
    margin-bottom: 1.5rem;
  }
</style>