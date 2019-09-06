<template>
  <div>
    <section class="md-layout md-gutter md-alignment-left">
      <AppProjectCard class="md-layout-item md-xlarge-size-20 md-large-size-25 md-medium-size-33 md-small-size-50 md-xsmall-size-100" 
      v-for='(project, index) in projects' :key='index'
      :id='project.id'
      :title='project.title'
      :client='project.client'
      :pm='project.projectManager'
      :team='project.selectedTeam'
      :description='project.description'
      :clients='clients'/>
    </section>
  </div>
</template>

<script>
export default {
 name: 'ProjectList',
 props: {
   projects: {
     type: Array,
     required: true
   },
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

    &:after {
      width: 100%;
      height: 100%;
      display: block;
      content: " ";
    }
}

</style>
