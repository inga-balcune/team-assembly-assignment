<template>
  <div>
    <md-toolbar md-elevation="0" class="md-primary">
      <a style="flex: 1" href="https://ingabalcune.dev/" target="blank">
        <h1 class="md-title">
          Signifly Assignment
          <span class="subtitle">by Inga Balcune</span>
        </h1>
      </a>
      <md-button to="/admin">Dashboard</md-button>
    </md-toolbar>
  </div>
</template>

<style lang='scss' scoped>
a {
  &:hover {
    text-decoration: none;
  }
}

.subtitle {
  display: block;
  margin-top: -0.8rem;
  font-size: 1.6rem;
  font-weight: 700;

  @include respond(phone) {
    margin-top: -1rem;
  }
}

.md-toolbar.md-theme-default.md-primary {
  background-color: $color-signifly-dark-blue;
  margin-bottom: 2rem;
}
</style>
