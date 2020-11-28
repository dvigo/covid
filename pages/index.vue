<template lang="pug">
  v-app
    Navbar
    Dashboard
</template>

<script>
  import Navbar from '~/components/Navbar/index.vue'
  import Dashboard from '~/components/Dashboard/index.vue'

  export default {
    layout: 'app',
    components: {
      Navbar,
      Dashboard
    }
  }
</script>