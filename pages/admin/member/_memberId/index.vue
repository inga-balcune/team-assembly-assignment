<template>
  <div>
    <AppEmployeeForm :employee="loadedEmployee" :sending='sending' @submit='onSubmitted'/>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sending: false
    }
  },
  async asyncData({ $axios, params }) {
    const data = await $axios.$get(`https://signifly-assignment.firebaseio.com/team/${params.memberId}.json`)
    return {loadedEmployee: data}
  },
  methods: {
    onSubmitted(loadedEmployee) {
      this.sending = true
      return this.$axios.$put(`https://signifly-assignment.firebaseio.com/team/${this.$route.params.memberId}.json`, loadedEmployee)
      .then(() => {
        this.sending = false
        this.$router.replace('/admin')
      })
      .catch(e => this.$router.replace('/error'))
    }
  }
}
</script>

<style lang="scss">
  
</style>