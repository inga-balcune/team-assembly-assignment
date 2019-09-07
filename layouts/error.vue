<template>
  <div>
    <md-empty-state
      md-icon="block"
      md-label="Sorry"
      md-description="Seems like something went wrong.">
      <md-button to="/admin" class="md-primary md-raised">Back to safety</md-button>
    </md-empty-state>
  </div>
</template>
<script>
export default {
  layout: 'admin'
}
</script>

<style lang='scss' scoped>

div {
    text-align: center;
  }

  .md-empty-state {
    display: inline-block;
    vertical-align: middle;

    + .md-empty-state {
      margin-left: 16px;
    }
  }

</style>
