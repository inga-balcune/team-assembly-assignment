<template>
  <div>
    <md-card>
      <md-card-header>
        <md-card-header-text>
          <div class="md-title">{{name}} {{surname}}</div>
          <div class="md-subhead">{{designation}}</div>
        </md-card-header-text>

        <md-card-media v-if='image'>
          <img :src="image" :alt="`${name} ${surname} image`">
        </md-card-media>
      </md-card-header>

      <md-card-actions>
        <md-button @click='deleteMember'>Delete</md-button>
        <md-button :to='memberLink()'>Edit</md-button>
        <md-button :to="`/team/${name.toLowerCase()}-${surname.toLowerCase()}`">Preview</md-button>
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
      designation: {
        type: String,
        required: true
      },
      image: {
        // type: String,
        // required: true
      },
      id: {
        type: String,
        required: true
      }
    },
  methods: {
    deleteMember() {
      if (window.confirm("Are you sure you want to delete this employee?")) { 
        return this.$axios.$delete(`https://signifly-assignment.firebaseio.com/team/${this.id}.json`, { key: this.id })
        .then(response => {
          this.$router.go(0)
          })
      }    
    },
    memberLink() {
      return `admin/member/${this.id}`

    }
  }
}
</script>

<style lang="scss">


</style>