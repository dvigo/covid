<template lang="pug">
  v-container(fluid style="padding: 0;")
    v-row(no-gutters)
      v-col(sm="12" style="position: relative;")
        div.chat-container(ref="chatContainer" :style="{backgroundImage: `url(${coronaJPG})`}")
          div.message(v-for="(message,index) in messages" v-bind:key="index" :class="{own: message.user == username}")
            div(class="username" v-if="index>0 && messages[index-1].user != message.user") {{message.user}}
            div(class="username" v-if="index == 0") {{message.user}}
            div(style="margin-top: 5px")
              div.content
                div(v-html="message.content")
        div.typer
          input(type="text" placeholder="Escribe aquí.." v-on:keyup.enter="sendMessage" v-model="message")
</template>

<script>
import coronaJPG from '~/assets/img/corona.jpg'

export default {
  data () {
    return {
      messages: [
      ],
      username: 'Tú',
      coronaJPG,
      message: ''
    }
  },
  methods: {
    async sendMessage () {
      let message = {
        user: 'Tú',
        content: this.message
      }
      this.messages.push(message)
      message = {
        user: 'CovidBot',
        content: ''
      }
      this.$axios.$get('/chatbot?message=' + this.message).then( this.message = '' ).then( (res) => (message.content = res.response)).then(this.messages.push(message))
    },
  }
}
</script>