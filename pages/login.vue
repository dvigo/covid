<template lang="pug">
  v-app
    Navbar
    Login
</template>

<script>
  import Navbar from '~/components/Navbar/index.vue'
  import Login from '~/components/Login/index.vue'

  export default {
    layout: 'app',
    components: {
      Navbar,
      Login
    }
  }
</script>