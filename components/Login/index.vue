<template lang="pug">
  v-flex(class='text-xs-center login') 
    .display-1.mb-3 #[v-icon.mr-2(large) work] MyWorkspace
    v-card(light)
      v-card-text
        .subheading
          template(v-if='true') Log in to your customer portal
          template(v-else) Crate a new account
        v-form
          input.input(placeholder="Tu correo electrónico" type="email" required ref="email")
          input.input(placeholder="Tu contraseña" type="password" required ref="password")
          v-btn(@click.prevent='login' block type='submit') Sign in
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
        }).then((res) => console.log(res.data.user))
      } catch (e) {
        this.error = e.response.data.message
      }
    },
  }
}
</script>