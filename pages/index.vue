<template>
  <div>
    <section class="section">
      <div class="hero">
        <div class="item-1">
          <div class="columns p-6">
            <div
              style="margin-top: auto"
              class="is-flex-direction-column column is-half"
            >
              <h1 class="title is-1">Stars of Heaven!</h1>
              <p pt-1 class="is-size-3 has-text-weight-light">
                Reverence Studios 2021 Recital
              </p>
              <br />
              <div v-if="isStreamOnly" class="buttons is-left">
                <nuxt-link
                  to="watch"
                  class="button is-medium is-black is-primary is-light"
                >
                  Watch
                </nuxt-link>
              </div>
              <div v-else-if="isDownloadOnly" class="buttons is-left">
                <nuxt-link
                  to="download"
                  class="button is-medium is-black is-primary is-light"
                >
                  Get Downloads
                </nuxt-link>
              </div>
              <div v-else-if="isStreamDownload" class="buttons is-left">
                <nuxt-link
                  to="watch"
                  class="button is-medium is-black is-primary is-light"
                >
                  Watch
                </nuxt-link>
                <nuxt-link
                  to="download"
                  class="button is-medium is-black is-primary is-light"
                >
                  Get Downloads
                </nuxt-link>
              </div>
              <div v-else-if="hasNoPlan" class="buttons is-left">
                <nuxt-link
                  to="/quick-pick-plan"
                  class="button is-medium is-black is-primary is-light"
                >
                  Pick A Plan
                </nuxt-link>
              </div>
              <div v-else class="buttons is-left">
                <nuxt-link
                  to="signup"
                  class="button is-medium is-black is-primary is-light"
                >
                  Sign up to Watch
                </nuxt-link>
              </div>
            </div>
            <div class="column is-half">
              <figure class="image is is-covered">
                <img src="~/assets/images/dancers.png" alt="" />
              </figure>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters(['isAuthenticated', 'loggedInUser']),
    isStreamOnly() {
      return (
        this.isAuthenticated &&
        this.loggedInUser?.role?.name === 'streaming-only'
      )
    },
    isDownloadOnly() {
      return (
        this.isAuthenticated &&
        this.loggedInUser?.role?.name === 'download-only'
      )
    },
    isStreamDownload() {
      return (
        this.isAuthenticated &&
        this.loggedInUser?.role?.name === 'stream-download'
      )
    },
    hasNoPlan() {
      return (
        this.isAuthenticated &&
        this.loggedInUser?.role?.name === 'Authenticated'
      )
    },
  },
}
</script>
