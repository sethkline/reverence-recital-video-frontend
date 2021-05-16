<template>
  <div class="container">
    <section class="section">
      <h1 class="title is-1">Purchase Tickets</h1>
      <label class="label">Select Show</label>
      <div class="field has-addons">
        <p class="control">
          <span class="select is-fullwidth" v-if="shows && shows.length">
            <select v-model="selectedShow">
              <option :value="null">Select dropdown</option>
              <option
                v-for="show in availableShows"
                :key="show.id"
                :value="show"
              >
                {{ show.description }}
              </option>
            </select>
          </span>
        </p>
        <p class="control">
          <button @click="addShow" type="button" class="button is-info">
            Add
          </button>
        </p>
      </div>
      <div class="p-4" v-if="selectedShows.length">
        <div
          v-for="show in selectedShows"
          :key="show.id"
          class="field has-addons"
        >
          <label class="label px-4">{{ show.description }}</label>

          <label class="label px-2">Quantity</label>
          <p class="control">
            <input
              class="input"
              @change="addToCart($event, show)"
              type="number"
            />
          </p>
          <p class="control">
            <button
              v-if="cart.some((item) => item.id === show.id)"
              @click="removeShow(show.id)"
              type="button"
              class="button is-danger"
            >
              &#10005;
            </button>
          </p>
        </div>
      </div>
      <div v-if="cart.length" class="p-4">
        <h2 class="title is-3">Subtotal:</h2>
        <p class="title is-2">{{ subTotal }}</p>
        <div class="field"></div>
        <button @click="goToCheckout" class="button is-primary">
          Proceed to Checkout
        </button>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get('/shows')
    return { shows: data }
  },
  data() {
    return {
      selectedShow: null,
      selectedShows: [],
      order: null,
      cart: [],
    }
  },
  computed: {
    subTotal() {
      if (this.cart.length) {
        const sumOfItems = this.cart
          .map((item) => {
            return item.ticketPrice * item.quanity
          })
          .reduce((a, b) => a + b, 0)
        return this.formatMoney(sumOfItems)
      } else {
        return this.formatMoney(0)
      }
    },
    availableShows() {
      if (this.selectedShows.length) {
        return this.shows.filter((show) => {
          return (
            this.selectedShows.some((selectedOption) => {
              return selectedOption.description === show.description
            }) === false
          )
        })
      } else return this.shows
    },
  },
  methods: {
    formatMoney(number) {
      return '$' + number
    },
    addShow() {
      if (this.selectedShow) {
        this.selectedShows.push(this.selectedShow)
        this.selectedShow = null
      }
    },
    addToCart(event, selectedShow) {
      const quanity = event.target.value
      // let selectedShow = this.shows.find(
      //   (show) => show.id === selectedShow.id
      // )
      const findShow = this.cart.find((show) => show.id === selectedShow.id)

      if (findShow) {
        findShow.quanity = quanity
      } else {
        this.cart.push({ quanity, ...selectedShow })
      }
    },
    removeById(id, array) {
      return array.filter((item) => {
        return item.id !== id
      })
    },
    removeShow(id) {
      this.cart = this.removeById(id, this.cart)
      this.selectedShows = this.removeById(id, this.selectedShows)
    },
    goToCheckout() {
      this.$router.push('/checkout')
    },
  },
}
</script>

<style lang="scss" scoped></style>
