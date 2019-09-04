<template>
  <div>
    <div class='card-container' :class="{teamTheme : title === 'Team Members', clientTheme : title === 'Clients', }">
      <md-icon class="md-size-2x">{{icon}}</md-icon>
      <h2 class='card-container__count'>{{count}}</h2>
      <h3 class='card-container__title'>{{title}}</h3>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true
    },
    count: {
      type: Number,
      required: true
    },
    icon: {
      type: String,
      required: true
    }
  }
   
}
</script>

<style lang="scss">

.card-container {
  display: block;
  background-color: $color-signifly-blue;
  padding: 3rem;
  margin-bottom: 2rem;
  text-align: right;
  
  &__count {
    display: inline-block;
    margin: 0 0;
    font-size: 5rem;
    font-weight: 900;
  }

  &__title {
    display: inline-block;
    margin: 0 .2rem;
    text-transform: uppercase;
  }
}

.md-icon.md-size-2x {
  display: block;
  margin: 0 0;
}

.teamTheme {
  background-color: $color-signifly-yellow;
}

.clientTheme {
  background-color: $color-signify-pink;
}

</style>