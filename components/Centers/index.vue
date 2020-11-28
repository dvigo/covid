<template lang="pug">
  div
    v-form.login(@submit="add()")
      input.input(placeholder="Localización" type="text" v-model="medicalCenter.location" ref="location")
      input.input(placeholder="Nombre" type="text" v-model="medicalCenter.name" ref="name")
      input.input(placeholder="Web" type="text" v-model="medicalCenter.website" ref="website")
      input.input(placeholder="Contacto" type="text" v-model="medicalCenter.contact_methods" ref="contact_methods")
      .button
        v-btn.font-weight-black(color='primary' type="submit") Enviar
    div(color="white")
      v-text-field.textbox(append-icon="search" label="Filter" single-line hide-details v-model="search" @keyup="filterCentros()")
      v-data-table(:headers="headers" :items="filterCentros()" class="elevation-1" light)
</template>

<script>
export default {
  data() {
    return {
      centros: [],
      search: '',
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
      this.centros = await this.$axios.$get('/medical_centers')
    },
    add () {
      event.preventDefault()
      this.$axios.$post('/medical_centers', this.medicalCenter)

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