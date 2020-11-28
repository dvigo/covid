<template lang="pug">
  v-form.login(@submit="login()")
    input.input(placeholder="Tu correo electrónico" type="email" required ref="email")
    input.input(placeholder="Tu contraseña" type="password" required ref="password")
    .button
      v-btn.font-weight-black(color='primary' type="submit") Enviar
</template>

<script>
export default {
  methods: {
    async login () {
      event.preventDefault()
      try {
        await this.$auth.loginWith('local', {
          data: {
            username: this.$refs.email.value,
            password: this.$refs.password.value
          }
        }).then((res) => console.log(res))
      } catch (e) {
        this.error = e.response.data.message
      }
    },
  }
}
</script>