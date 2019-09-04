<template>
  <div>
    <section class="md-layout md-gutter md-alignment-left">
      <AppEmployeeIdCard 
      class="md-layout-item md-xlarge-size-25 md-large-size-33 md-medium-size-33 md-small-size-50 md-xsmall-size-100"
      :class="{ 'md-layout-item md-xlarge-size-100 md-large-size-100 md-medium-size-100 md-small-size-100 md-xsmall-size-100': team.length === 1 }" 
      v-for='(member, index) in team' :key='index'
      :id='member.id'
      :name='member.firstName'
      :surname='member.lastName'
      :designation='member.designation'
      :image='member.image'
      :number='member.number'
      :email='member.email'/>
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
