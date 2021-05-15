<template>
  <section class="section">
    <div class="container">
      <div class="has-text-centered" id="services-text-container">
        Welcome {{ loggedInUser.username }}
        <h1 class="title is-1">Our Shows</h1>
        <h4 class="subtitle">
          The Videos will be posted when they are available
        </h4>
      </div>
      <br />
      <div class="columns">
        <div class="column">
          <div class="card">
            <div class="card-content">
              <div class="has-text-centered">
                <img src="images/card-item-1.png" />
              </div>
              <h3
                class="title is-3 has-text-centered"
                id="card-product-description"
              >
                May 17th
              </h3>
              <p class="has-text-centered">
                There are many variations of passages of Lorem Ipsum available,
                but the majority have suffered alteration in some form, by
                injected humour, or randomised words which don't look even
                slightly believable.
              </p>
            </div>
          </div>
        </div>
        <div class="column">
          <div class="card">
            <div class="card-content">
              <div class="has-text-centered">
                <img src="images/card-item-1.png" />
              </div>
              <h3
                class="title is-3 has-text-centered"
                id="card-product-description"
              >
                May 18th
              </h3>
              <p class="has-text-centered">
                There are many variations of passages of Lorem Ipsum available,
                but the majority have suffered alteration in some form, by
                injected humour, or randomised words which don't look even
                slightly believable.
              </p>
            </div>
          </div>
        </div>
        <div class="column">
          <div class="card">
            <div class="card-content">
              <div class="has-text-centered">
                <img src="images/card-item-1.png" />
              </div>
              <h3
                class="title is-3 has-text-centered"
                id="card-product-description"
              >
                May 19th
              </h3>
              <p class="has-text-centered">
                There are many variations of passages of Lorem Ipsum available,
                but the majority have suffered alteration in some form, by
                injected humour, or randomised words which don't look even
                slightly believable.
              </p>
            </div>
          </div>
        </div>
        <div v-if="videos && videos.length">
          <test-video
            v-for="video in videos"
            :key="video.id"
            :streamLink="video.mux_video_uploader_mux_asset.playback_id"
          />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  middleware: 'auth',
  name: 'Watch',
  async asyncData({ $axios }) {
    const { data } = await $axios.get('/videos')
    return { videos: data }
  },
  computed: {
    ...mapGetters(['loggedInUser']),
  },
}
</script>
