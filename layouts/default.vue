<template lang="pug">
  v-app(light)
    v-app-bar(:clipped-left="clipped" fixed app)
      v-toolbar-title(v-text="title")
      v-spacer
      v-btn(icon @click.stop="rightDrawer = !rightDrawer")
        v-icon mdi-menu
    v-main
      v-container
        nuxt
    v-navigation-drawer(v-model="rightDrawer" :right="right" temporary fixed)
      v-list
        v-list-item(v-if="!isAuthenticated" href="login")
          v-list-item-icon
            v-icon mdi-account
          v-list-item-content Acceder
        v-list-item(v-for="item in items" :href="item.action" :target="(item.target) ? item.target : ''")
          v-list-item-icon(v-if="item.icon")
            v-icon {{ item.icon }}
          v-list-item-content {{ item.title}}
        v-list-item(v-if="isAuthenticated" @click="logout")
          v-list-item-icon
            v-icon mdi-run-fast
          v-list-item-content Salir
    v-footer(
      :absolute="!fixed"
      app
    )
      span &copy; {{ new Date().getFullYear() }}
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          title: 'Estadísticas',
          action: 'dashboard',
          icon: 'mdi-chart-bell-curve'
        },
        {
          title: 'Notícias',
          action: 'news',
          icon: 'mdi-newspaper-variant'
        },
        {
          title: 'Encuentra tu centro',
          action: 'centros',
          icon: 'mdi-hospital-building'
        },
        {
          title: 'Preguntas Frecuentes',
          action: 'faqs',
          icon: 'mdi-comment-question'
        },
        {
          title: 'CovidBot',
          action: 'bot',
          icon: 'mdi-robot'
        },
        {
          title: 'Contacta',
          action: 'https://api.whatsapp.com/send?phone=553432424&text=Covid%20Information',
          icon: 'mdi-whatsapp',
          target: "_blank"
        },
        {
          title: 'RadarCovid',
          action: 'radar',
          icon: 'mdi-radar'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Covid4U'
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  methods: {
    logout () {
      this.$auth.logout()
    }
  }
}
</script>
