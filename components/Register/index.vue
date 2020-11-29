<template lang="pug">
  v-flex(class='text-xs-center')
    v-spacer
    v-container(style="max-width: 500px; min-height: 500px") 
      .display-1.mb-3.text-center #[v-icon.mr-2(large) mdi-virus] MyCovid
      v-card(light style="min-height: 200px")
        v-card-text
          v-form(@submit='signup')
            v-text-field.input(placeholder="Tu nombre de usuario" type="text" required v-model="username")
            v-text-field.input(placeholder="Tu nombre" type="text" required v-model="name")
            v-text-field.input(placeholder="Tu email" type="email" required v-model="email")
            v-text-field.input(placeholder="Tu contraseña" type="password" required v-model="password")
            v-spacer
            v-text-field.input(placeholder="Repite la contraseña" type="password" required v-model="password2")
            v-spacer
            v-btn(block type='submit') Sign up
          p.iserror {{ this.error }}
</template>

<script>
export default {
  data () {
    return {
      error: '',
      username: '',
      password: '',
      password2: '',
      name: '',
      email: ''
    }
  },
  methods: {
    async signup () {
      event.preventDefault()
      this.error = ''
      try {
        let user = {
            email: this.email,
            username: this.username,
            password: this.password,
            name: this.name
        }
        if(this.password == this.password2){
            await this.$axios.$post('/register', user).then((res) => this.$router.push('/login'))
        }else{
            this.error = 'Las contraseñas no coinciden'
        }
        
      } catch (e) {
        this.error = 'Usuario o contraseña incorrectos'
      }
    },
  }
}
</script>