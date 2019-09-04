<template>
  <div class="card-expansion">

    <md-card md-elevation-0>
      <md-card-media md-ratio="4:3">
        <img :src="image" :alt="`${name} ${surname} photo`">
      </md-card-media>
      
      <md-card-header>
        <div class="md-title">{{name}} {{surname}}</div>
        <div class="md-subhead">{{designation}}</div>
      </md-card-header>
      
      <md-card-content>
        <div class='icon-container'>
          <md-icon>phone_iphone</md-icon>
          <span><a :href="`tel:+45${number}`">+45 {{number.length === 8 ? number.match(/.{1,4}/g).join(' ') : number}}</a></span>
        </div>
        <div class='icon-container'>
          <md-icon>email</md-icon>
          <span><a :href="`mailto:${email}`">{{email}}</a></span>
        </div>
        
      </md-card-content>

     

      

      <md-card-actions>
        <md-button :to="`/team/${name.toLowerCase()}-${surname.toLowerCase()}`">Profile</md-button>
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
        type: String,
        required: true
      },
      id: {
        type: String,
        required: true
      },
      number: {
        type: String,
        required: true
      },
      email: {
        type: String,
        required: true
      }
    },
  methods: {
    deleteMember() {
      return this.$axios.$delete(`https://signifly-assignment.firebaseio.com/team/${this.id}.json`, { key: this.id })
      .then(response => {
        this.$router.go(0)
        })
    },
    memberLink() {
      return `admin/member/${this.id}`

    }
  }
}
</script>

<style lang="scss">

  .icon-container {
    margin: .5rem 0;
  }
  
  .md-card-media.md-ratio-4-3:before {
    padding-top: 69%;
  }


  .md-card-media.md-ratio-4-3 img {
    top: 90%;
  }

  
</style>