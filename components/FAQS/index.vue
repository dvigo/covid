<template lang="pug">
  div
    v-dialog( v-model="dialog" scrollable hide-overlay transition="dialog-bottom-transition")
      template(v-slot:activator="{ on, attrs }")
        v-btn.ask(v-if="!loggedInUser.is_expert" color="primary" dark v-bind="attrs" v-on="on" )
           | Preguntar
        h2 Preguntas Frecuentes
      v-card
        v-card-title
          | Preguntar
        v-card-text
          v-form(@submit="add()")
            input.input(placeholder="Título" type="text" v-model="faq.title" ref="location")
            v-textarea.input(placeholder="Question" v-model="faq.question" ref="name")
            v-checkbox(v-model="faq.anon" label="Anónimo")
        v-card-actions
          v-spacer
          v-btn(color="blue darken-1" text @click="add()")
           | Enviar
    div.question(v-for="faq in faqs")
      h4 {{ faq.title }}
      p {{ faq.question }}
      v-dialog( v-model="dialog" scrollable hide-overlay transition="dialog-bottom-transition")
        template(v-slot:activator="{ on, attrs }")
          v-btn(v-if="loggedInUser.is_expert && faq.answers.length == 0" v-bind="attrs" v-on="on" )
            | Responder
        v-card
          v-card-title
            | Añadir respuesta
          v-card-text
            v-form(@submit="add()")
              v-textarea.input(placeholder="Question" v-model="answer.answer" ref="name")
              v-checkbox(v-model="faq.anon" label="Anónimo")
          v-card-actions
            v-spacer
            v-btn(color="blue darken-1" text @click="addAnswer(faq.id)")
              | Enviar
      div.answer(v-for="answer in faq.answers")
        v-rating(v-if="loggedInUser" v-model="rating" color="yellow darken-3" background-color="grey darken-1" empty-icon="$ratingFull" half-increments hover @change="rate(answe.id)")
        p.answerRight.bold {{ answer.user.username }}:
        p.answerRight {{ answer.answer }}

        
    v-btn(@click="logout()") SALIR
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  data() {
    return {
      dialog: false,
      faqs: [],
      faq: {
        tile: '',
        question: '',
        anon: false
      },
      answer: {
        answer: '',
        question: null
      },
      rating: 0
    }
  },
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser'])
  },
  created () {
    this.fetch()
  },
  methods: {
    async fetch () {
      this.faqs = await this.$axios.$get('/questions')
    },
    add () {
      event.preventDefault()
      this.$axios.$post('/questions', this.faq).then( this.faqs.push(this.faq))
    },
    addAnswer (id) {
      console.log(id)
      this.answer.question = id
      this.$axios.$post('/questions/answers', this.answer).then( this.answer.question = null )
    },
    logout () {
      this.$auth.logout();
    },
    rate (id) {
      console.log(this.rating)
      // this.$axios.$post('/questions/answers/rate', ).then( this.rating = 0 )
    }
  }
}
</script>