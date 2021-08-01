<template>
  <b-navbar toggleable="lg" type="dark" variant="primary" sticky>
    <b-navbar-brand to="/">Natlex</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item to="/">Home</b-nav-item>
        <b-nav-item to="/charts">Charts</b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
</template>

<script>
  export default {
    name: "TheHeader"
  }
</script>

<style scoped>

</style>
