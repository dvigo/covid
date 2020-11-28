<template lang="pug">
  v-app
    Navbar
    Centers
</template>

<script>
  import Navbar from '~/components/Navbar/index.vue'
  import Centers from '~/components/Centers/index.vue'

  export default {
    layout: 'app',
    components: {
      Navbar,
      Centers
    }
  }
</script>