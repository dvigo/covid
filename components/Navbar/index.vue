<template lang="pug">
  div
    v-app-bar(light fixed app)
      v-toolbar-title.title COVID-19
      v-spacer
      v-toolbar-items.hidden-sm-and-down
        v-btn(href="login") Entrar
        v-btn(href="centros") Busca tu centro
      v-toolbar-items.hidden-md-and-up
        v-app-bar-nav-icon(@click="drawer = true")
    v-navigation-drawer(v-model="drawer" absolute temporary right)
      v-list(nav dense)
        v-list-item-group( v-model="group" active-class="deep-purple--text text--accent-4")
          v-list-item(href="centros")
            v-list-item-icon
              v-icon mdi-home
            v-list-item-title Busca tu centro
</template>

<script>
  export default {
    data() {
      return {
        drawer: false,
        group: null,
      }
    },
    watch: {
      group () {
        this.drawer = false
      },
    },
  }
</script>