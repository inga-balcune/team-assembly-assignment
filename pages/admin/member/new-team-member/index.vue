<template>
  <div>
    <AppEmployeeForm @submit='onSubmitted' :sending='sending'/>
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
    onSubmitted(formData) {
    this.sending = true
    return axios.post(`https://signifly-assignment.firebaseio.com/team.json`, formData)
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