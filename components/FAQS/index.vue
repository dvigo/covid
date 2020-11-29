<template lang="pug">
  div
    v-dialog( v-model="dialog" scrollable hide-overlay transition="dialog-bottom-transition")
      template(v-slot:activator="{ on, attrs }")
        v-btn.ask(v-if="checkUserNoExpert()" color="primary" dark v-bind="attrs" v-on="on" )
           | Preguntar
        h2 Preguntas Frecuentes
      v-card
        v-card-title
          | Preguntar
        v-card-text
          v-form(@submit="add()")
            input.input(placeholder="Título" type="text" v-model="faq.title" ref="location")
            v-textarea.input(placeholder="Question" v-model="faq.question" ref="name")
            v-checkbox(v-if="isAuthenticated" v-model="faq.anon" label="Anónimo")
        v-card-actions
          v-spacer
          v-btn(color="blue darken-1" text @click="add()")
           | Enviar
    div.question(v-for="faq in faqs")
      h4 {{ faq.title }}
      p {{ faq.question }}
      v-dialog(v-if="isAuthenticated" v-model="dialog" scrollable hide-overlay transition="dialog-bottom-transition")
        template(v-slot:activator="{ on, attrs }")
          v-btn(v-if="isAuthenticated && loggedInUser.is_expert && faq.answers.length == 0" v-bind="attrs" v-on="on" )
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
        v-rating(@input="addRating($event, answer.id)" :value="answer.average_rating" background-color="orange lighten-3" color="orange" :dense="true" :hover="true" size="15" :id="answer.id" half-increments)
        div.text-center.d-flex.align-right.justify-space-around
          v-spacer
          v-tooltip(top)
            template(v-slot:activator="{ on, attrs }")
              p.answerRight.bold(v-bind="attrs" v-on="on") {{ answer.user.username }}:
            span 
              v-rating(:value="answer.user.answer_rating" background-color="indigo lighten-3" color="indigo" :dense="true" :hover="true" size="15" :id="answer.user.id" half-increments readonly)
        p.answerRight {{ answer.answer }}
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  data() {
    return {
      dialog: false,
      faqs: [],
      faq: {
        title: '',
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
      this.$axios.$post('/questions', this.faq).then( this.faqs.push(this.faq)).then(this.$forceUpdate(), this.dialog = false)
    },
    addAnswer (id) {
      this.answer.question = id
      this.$axios.$post('/questions/answers', this.answer).then( this.answer.question = null ).then(this.$forceUpdate(), this.dialog = false)
    },
    logout () {
      this.$auth.logout();
    },
    addRating (props, id) {
      let answer = {
        answer: id,
        rating: props
      }
      this.$axios.$post('/questions/answers/rate', answer)
    },
    checkUserNoExpert () {
      if(!this.isAuthenticated) {
        return true
      }else if(this.loggedInUser.is_expert == false){
        return true
      }
      return false

    }
  }
}
</script>