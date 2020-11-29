<template lang="pug">
  v-container
    v-row(sm="12")
      h3 ¿Qué es Radar COVID?
      p Radar COVID es una aplicación móvil desarrollada para ayudar a controlar la propagación de la COVID-19 a
        | través de la identificación de los posibles contactos estrechos de casos confirmados a través de la tecnología
        | Bluetooth.
    v-row(sm="12")
      v-col(sm="9")
        v-img(:src="radarcovid")
      v-col(sm="3")
        a( href='https://apps.apple.com/es/app/radar-covid/id1520443509' target="_blank")
          v-img.apple(:src="apple")
        a( href="https://play.google.com/store/apps/details?id=es.gob.radarcovid&hl=es&gl=US" target="_blank")
          v-img.android(:src="android" href="https://play.google.com/store/apps/details?id=es.gob.radarcovid&hl=es&gl=US" target="_blank")

</template>

<script>
import radarcovid from '~/assets/img/radarcovid.jpg'
import android from '~/assets/img/android.png'
import apple from '~/assets/img/apple.png'

export default {
  data () {
      return {
        radarcovid,
        android,
        apple
      }
  }
}
</script>