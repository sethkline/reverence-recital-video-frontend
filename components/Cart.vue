<template>
  <div class="is-full" v-if="price > 0">
    <table class="table is-fullwidth is-hoverable">
      <thead>
        <tr>
          <th>Plan</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="selectedPlan.length">
          <td>{{ selectedPlan[0].name }}</td>
          <td>${{ selectedPlan[0].price }}</td>
          <td>
            <a
              ><span
                class="has-text-danger"
                @click="removeFromCart(selectedPlan)"
                >X</span
              ></a
            >
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  methods: {
    ...mapMutations({
      addToCart: 'cart/add',
      removeFromCart: 'cart/remove',
    }),
    goToCheckout() {
      // Redirect to signin page if not logged in.
      const isConnected = this.$store.getters['auth/username']
      if (!isConnected) {
        this.$router.push('/users/signin')
        return
      }
      this.$router.push('/orders/checkout')
    },
  },
  computed: {
    // id() {
    //   return this.$route.params.id
    // },
    selectedPlan() {
      return this.$store.getters['cart/items']
    },
    price() {
      return this.$store.getters['cart/price']
    },
    // numberOfItems() {
    //   return this.$store.getters['cart/numberOfItems']
    // },
    // ...mapState(['cart/plan']),
  },
}
</script>
