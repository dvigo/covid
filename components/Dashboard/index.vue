<template lang="pug">
  v-container
    v-row(sm="12" v-if="this.last_updated")
      div
        h1 Resumen
      div(style="width: 100%")
       | Generado automáticamente. Actualizado {{ new Date(this.last_updated.slice(0, 10)).toLocaleDateString('es')}}
      v-col(sm="6" v-if="this.spain.length != 0")
        v-simple-table
          tr
            td(valign="top" width="50%")
              h3 España en cifras
            td
              div 
                span(style="font-weight: bold") {{ this.spain.infected_today }}
                |  Infectados hoy
              div
                span(style="font-weight: bold") {{ this.spain.total_cases }}
                |  Casos Totales
              div 
                span(style="font-weight: bold") {{ this.spain.deceased_today }}
                |  Fallecidos hoy
              div 
                span(style="font-weight: bold") {{ this.spain.deceased_total }}
                |  Fallecidos Totales
          tr
            td(valign="top")
              h3 Comunidades con más casos
            td
              div(v-for="comunitie in this.spain.most_cases_comunities")
                span(style="font-weight: bold") {{ comunitie.cases}}
                |  {{ comunitie.name }}
      v-col(sm="6" v-if="this.worldwide.length != 0")
        v-simple-table
          tr
            td(valign="top" width="50%")
              h3 El mundo en cifras
            td
              div 
                span(style="font-weight: bold") {{ this.worldwide.infected_active }}
                |  Infectados activos
              div
                span(style="font-weight: bold") {{ this.worldwide.total_cases }}
                |  Casos Totales
              div 
                span(style="font-weight: bold") {{ this.worldwide.recovered }}
                |  Recuperados
              div 
                span(style="font-weight: bold") {{ this.worldwide.deceased }}
                |  Fallecidos Totales
          tr
            td(valign="top")
              h3 Países con más casos
            td
              div(v-for="country in this.worldwide.most_cases_countries")
                span(style="font-weight: bold") {{ country.cases}}
                |  {{ country.name }}
    v-row(sm="12")
      v-col(sm="6")
        v-img(:src="`https://resources.gabriele.ai/coronavirus/${url}-2_15-COVID-only_country-spain-es_COVID_ES_country_confirmed.png`")
      v-col(sm="6")
        v-img(:src="`https://resources.gabriele.ai/coronavirus/${url}-2_15-COVID-only_country-spain-es_COVID_ES_country_deaths.png`")
</template>

<script>
export default {
  data () {
    return {
      url: new Date().toISOString().slice(0,10),
      spain: [],
      worldwide: [],
      last_updated: null
    }
  },
  created ()  {
    this.fetch()
  },
  methods: {
    async fetch () {
      this.$axios.$get('/today').then((res) => {
        this.spain = res.spain
        this.worldwide = res.mr_worldwide
        this.last_updated = res.last_updated
      })
    }
  }
}
</script>