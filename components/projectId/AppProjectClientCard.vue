<template>
  <div>
    <div v-for='data in client' :key='data.id'>
      <div class='meta-item'>
        <div class='item-label'>Client:</div>
        <div>{{data.client}}</div>
      </div>
      <div class='meta-item'>
        <div class='item-label'>Project Lead:</div>
        <div>{{data.firstName}} {{data.lastName}}</div>
      </div>
      <div class='meta-item'>
        <div class='item-label'>Email:</div>
        <div><a :href="`mailto:${data.email}`">{{data.email}}</a></div>
      </div>
      <div class='meta-item'>
        <div class='item-label'>Phone:</div>
        <div><a :href="`tel:+45${data.number}`">+45 {{data.number.length === 8 ? data.number.match(/.{1,4}/g).join(' ') : data.number}}</a></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
      client: {
        required: true
      }
  },
}
</script>

<style lang="scss" scoped>

.meta-item {
  width: 100%;
  display: flex;
  flex-direction: row;
}

.item-label {
  font-weight: 500;
  margin-right: 1rem;
}
</style>