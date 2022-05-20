<template>
  <section class="section">
    <div class="container">
      <div class="columns">
        <div class="column is-4 is-offset-4">
          <h2 class="title has-text-centered">Create Account</h2>

          <Notification v-if="error" type="danger" :message="error" />

          <form v-if="!success" method="post" @submit.prevent="register">
            <div class="field">
              <label class="label">Username</label>
              <div class="control">
                <input
                  v-model="username"
                  type="text"
                  class="input"
                  name="username"
                  required
                />
              </div>
            </div>
            <div class="field">
              <label class="label">Email</label>
              <div class="control">
                <input
                  v-model="email"
                  type="email"
                  class="input"
                  name="email"
                  required
                />
              </div>
            </div>
            <div class="field">
              <label class="label">Password</label>
              <div class="control">
                <input
                  v-model="password"
                  type="password"
                  class="input"
                  name="password"
                  required
                />
              </div>
            </div>
            <div class="has-text-centered pt-4">
              <button type="submit" class="button is-primary is-large">
                Continue
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import Notification from '~/components/Notification'

export default {
  middleware: 'guest',
  components: {
    Notification,
  },
  data() {
    return {
      username: '',
      email: '',
      password: '',
      success: null,
      error: null,
    }
  },
  methods: {
    async register() {
      this.error = null
      try {
        this.$axios.setToken(false)
        await this.$axios.post('auth/local/register', {
          username: this.username,
          email: this.email,
          password: this.password,
        })
        this.success = `A confirmation link has been sent to your email account. \
 Please click on the link to complete the registration process.`
        const response = await this.$auth.loginWith('local', {
          data: {
            identifier: this.email,
            password: this.password,
          },
        })
        await this.$axios.setToken(response.jwt, 'Bearer', ['post', 'delete'])
        this.$router.push('/checkout')
      } catch (e) {
        this.error = e.response.data.message[0].messages[0].message
      }
    },
  },
}
</script>
