<template>
  <div class="container p-4">
    <div class="columns">
      <div v-if="plans && plans.length" class="column is-10 is-offset-1">
        <h1 class="title is-size-3">Pick a plan</h1>
        <div
          v-for="plan in plans"
          :key="plan.id"
          class="hero box p-0 pointer"
          :class="[
            selectedOption.slug === plan.slug ? 'is-primary' : 'is-black',
          ]"
          @click="selectedOption = plan"
        >
          <div class="hero-body">
            <p class="title">{{ plan.name }}</p>
            <p class="subtitle">${{ plan.price }} - {{ plan.description }}</p>
          </div>
        </div>
        <div v-if="selectedOption.slug" class="has-text-centered">
          <button class="button is-primary is-large" @click="setPlan">
            Continue
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get('/plans')
    return { plans: data }
  },
  data() {
    return {
      selectedOption: { slug: '' },
    }
  },
  methods: {
    ...mapMutations({
      addToCart: 'cart/add',
      clearCart: 'cart/emptyList',
    }),
    setPlan() {
      this.clearCart()
      this.addToCart(this.selectedOption)
      this.$router.push('/signup/create-account-info')
    },
  },
}
</script>
