<template>
  <div>
    <the-header></the-header>
    <b-container class="pb-5">
      <nuxt></nuxt>
    </b-container>
  </div>
</template>

<script>
  import TheHeader from "../components/TheHeader";

  export default {
    name: "default",
    components: {
      TheHeader
    }
  }
</script>

<style scoped>

</style>
