<template>
  <div>
    <md-empty-state
      class="md-primary"
      :md-icon="icon"
      :md-label="label"
      :md-description="description">
    </md-empty-state>
  </div>
</template>

<script>
export default {
  props: {
    icon: {
      type: String,
      required: true
    },
    label: {
      type: String,
      required: true
    },
    description: {
      type: String,
      required: true
    }
  },
  name: 'EmptyState'
}
</script>

<style lang="scss" scoped>
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