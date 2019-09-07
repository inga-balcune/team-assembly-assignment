<template>
  <div>
    <AppClientForm :client="loadedClient" :sending='sending' @submit='onSubmitted'/>
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
    const data = await $axios.$get(`https://signifly-assignment.firebaseio.com/client/${params.clientId}.json`)
    return {loadedClient: data}
  },
  methods: {
    onSubmitted(loadedClient) {
      this.sending = true
      return this.$axios.$put(`https://signifly-assignment.firebaseio.com/client/${this.$route.params.clientId}.json`, loadedClient)
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