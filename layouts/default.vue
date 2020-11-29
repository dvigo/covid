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
        v-list-item(v-for="item in items" :href="item.action")
          v-list-item-content {{ item.title}}
          v-list-item-icon(v-if="item.icon")
            v-icon {{ item.icon }}
    v-footer(
      :absolute="!fixed"
      app
    )
      span &copy; {{ new Date().getFullYear() }}
</template>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          title: 'Encuentra tu centro',
          action: 'centros'
        },
        {
          title: 'Preguntas Frecuentes',
          action: 'faqs'
        },
        {
          title: 'Contacta',
          action: 'https://api.whatsapp.com/send?phone=553432424&text=Covid%20Information',
          icon: 'mdi-whatsapp'
        },
        {
          title: 'RadarCovid'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Covid4U'
    }
  }
}
</script>
