<template>
  <div>
    <AppClientForm @submit='onSubmitted' :sending='sending'/>
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
  methods: {
    onSubmitted(clientData) {
    this.sending = true
    return axios.post(`https://signifly-assignment.firebaseio.com/client.json`, clientData)
    .then(() => {
      this.sending = false
      this.$router.push('/admin')
    })
   }
  }
}
</script>

<style lang="scss">
  
</style>