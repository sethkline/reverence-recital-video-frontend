<template>
  <div>
    <a class="button button-primary m-3" @click="$router.go(-1)">
      <span icon="arrow-left"></span> go back</a
    >
    <section class="section">
      <div class="columns">
        <div class="column is-4 is-offset-4">
          <form @submit.stop.prevent="handleSubmit">
            <h1 class="title">Payment Method</h1>
            <p class="control p-4">
              <label for="full-name" class="form-label">Full name</label>
              <input
                id="full-name"
                v-model="fullName"
                type="text"
                class="input p-2"
                required
              />
            </p>
            <p class="control p-4">
              <label class="form-label" for="address">Address</label>
              <input
                id="address"
                v-model="address"
                class="input"
                type="text"
                required
              />
            </p>

            <p class="control p-4">
              <label class="form-label" for="city">City</label>
              <input
                id="city"
                v-model="city"
                class="input"
                type="text"
                required
              />
            </p>

            <p class="control p-4">
              <label class="form-label" for="postal-code" required
                >Postal code</label
              >
              <input
                id="postal-code"
                v-model="postalCode"
                class="input"
                type="text"
              />
            </p>
            <div class="p-4">
              <card
                ref="card-stripe"
                class="box"
                :stripe="stripeKey"
                :options="stripeOptions"
                @change="complete = $event.complete"
              />
            </div>
            <div class="p-4">
              <Cart class="p-4" />
            </div>
            <div class="has-text-centered pt-4">
              <button class="button is-primary is-large">
                Proccess Payment(${{ price }})
              </button>
            </div>
          </form>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import { Card, createToken } from 'vue-stripe-elements-plus'

export default {
  middleware: 'auth',
  name: 'Checkout',
  components: {
    Card,
  },
  computed: {
    price() {
      return this.$store.getters['cart/price']
    },
    selectedPlan() {
      return this.$store.getters['cart/items']
    },
    stripeKey() {
      return process.env.STRIPE_KEY
    },
  },
  data() {
    return {
      address: '',
      postalCode: '',
      city: '',
      fullName: '',
      loading: false,
      successRoute: {
        'streaming-only': '/watch',
        'stream-download': '/watch',
        'download-only': '/download',
      },
      stripeOptions: {
        style: {
          base: {
            iconColor: '#c4f0ff',
            color: '#fff',
            fontWeight: '500',
            fontFamily: 'Roboto, Open Sans, Segoe UI, sans-serif',
            fontSize: '16px',
            fontSmoothing: 'antialiased',
            ':-webkit-autofill': {
              color: '#080800',
            },
            '::placeholder': {
              color: '#bebebe',
            },
          },
          invalid: {
            iconColor: '#FFC7EE',
            color: '#FFC7EE',
          },
        },
        focus: false,
      },
    }
  },
  methods: {
    async handleSubmit() {
      this.loading = true
      let token
      try {
        const response = await createToken()
        token = response.token.id
      } catch (err) {
        alert('An error occurred.')
        this.loading = false
        return
      }
      const prices = {
        amount: this.price,
        plan: this.selectedPlan,
        fullName: this.fullName,
        address: this.address,
        postalCode: this.postalCode,
        city: this.city,
        token,
      }
      try {
        await this.$axios.post('orders', prices)
        alert('Your order have been successfully submitted.')
        this.updateAuthUserRole()
        const route = this.successRoute[this.selectedPlan[0].slug] || '/'
        this.$router.push(route)
      } catch (err) {
        this.loading = false
        alert('An error occurred.')
      }
    },
    updateAuthUserRole() {
      const updatedUser = { ...this.$auth.user }
      updatedUser.role.name = this.selectedPlan[0].slug
      this.$auth.setUser(updatedUser)
    },
  },
}
</script>
