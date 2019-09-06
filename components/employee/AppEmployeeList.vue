<template>
  <div>
    <section class="md-layout md-gutter md-alignment-left">
      <AppEmployeeCard class="md-layout-item md-xlarge-size-20 md-large-size-25 md-medium-size-33 md-small-size-50 md-xsmall-size-100" 
      v-for='(member, index) in team' :key='index'
      :id='member.id'
      :name='member.firstName'
      :surname='member.lastName'
      :designation='member.designation'
      :image='member.image'/>
    </section>
  </div>
</template>

<script>
export default {
 name: 'EmployeeList',
 props: {
   team: {
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

    &:after {
      width: 100%;
      height: 100%;
      display: block;
      content: " ";
    }
  }

</style>
