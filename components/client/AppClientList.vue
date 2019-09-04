<template>
  <div>
    <section class="md-layout md-gutter md-alignment-left">
      <AppClientCard class="md-layout-item md-xlarge-size-20 md-large-size-25 md-medium-size-33 md-small-size-50 md-xsmall-size-100" 
      v-for='(client, index) in clients' :key='index'
      :id='client.id'
      :name='client.firstName'
      :surname='client.lastName'
      :client='client.client'
      :logo='client.logo'/>
    </section>
  </div>
</template>

<script>
export default {
 name: 'ClientList',
 props: {
   clients: {
     type: Array,
     required: true
   }
 }
}
</script>

<style lang='scss' scoped>

.md-layout-item {
    margin-top: .8rem;
    margin-bottom: .8rem;
    // transition: .3s $md-transition-stand-timing;

    &:after {
      width: 100%;
      height: 100%;
      display: block;
      // background: md-get-palette-color(purple, 200);
      content: " ";
    }
}

</style>
