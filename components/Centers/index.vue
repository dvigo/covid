<template lang="pug">
  div
    v-dialog( v-model="dialog" max-width="600px" transition="dialog-bottom-transition")
      template(v-slot:activator="{ on, attrs }")
        v-btn.ask(color="primary" dark v-bind="attrs" v-on="on" )
           | Añadir
        h2 Centros
      v-card
        v-card-title
          | Preguntar
        v-card-text
          v-form(@submit="add()")
            v-text-field.input(placeholder="Localización" type="text" v-model="medicalCenter.location" ref="location")
            v-text-field.input(placeholder="Nombre" type="text" v-model="medicalCenter.name" ref="name")
            v-text-field.input(placeholder="Web" type="text" v-model="medicalCenter.website" ref="website")
            v-text-field.input(placeholder="Contacto" type="text" v-model="medicalCenter.contact_methods" ref="contact_methods")
        v-card-actions
          v-spacer
          v-btn(color="blue darken-1" text @click="add()")
           | Añadir
    div(color="white")
      v-text-field.textbox(append-icon="search" label="Filter" single-line hide-details v-model="search" @keyup="filterCentros()")
      v-data-table(:headers="headers" :items="filterCentros()" class="elevation-1" light :loading="loading")
</template>

<script>
export default {
  data() {
    return {
      centros: [],
      search: '',
      dialog: false,
      loading: true,
      medicalCenter: {
        location: null,
        name: null,
        website: null,
        contact_methods: null
      },
      headers: [
        { text: 'Población', value: 'location' },
        { text: 'Nombre', value: 'name' },
        { text: 'Web', value: 'web' },
        { text: 'Contacto', value: 'contact_methods' }
      ]
    }
  },
  created () {
    this.fetch()
  },
  methods: {
    async fetch () {
      this.centros = await this.$axios.$get('/medical_centers').then(this.loading = false)
    },
    add () {
      event.preventDefault()
      this.$axios.$post('/medical_centers', this.medicalCenter).then(this.$forceUpdate()).then((res) => {
        this.centros.push(this.medicalCenter)
      }).then(this.dialog = false)
    },
    filterCentros() {
      return this.centros.filter(c => {
        if(c.location.toLowerCase().includes(this.search.toLowerCase()) || c.name.toLowerCase().includes(this.search.toLowerCase())){
          return true
        }else{
          return false
        }
      })
    }
  }
}
</script>