<template>
  <section class="section">
    <div class="container">
      <div class="has-text-centered" id="services-text-container">
        Welcome {{ loggedInUser.username }}
        <h1 class="title is-1">Reverence Studios Recital Videos</h1>
        <h4 class="subtitle" v-if="videos.length === 0">
          There are no videos to stream yet check back later.
        </h4>
      </div>
      <br />
      <div class="columns">
        <div class="column" v-if="videos && videos.length">
          <div class="card" v-for="video in videos" :key="video.id">
            <div class="card-content">
              <div class="has-text-centered">
                <figure class="image is-16by9">
                  <img
                    :src="[
                      createThumbnail(
                        video.mux_video_uploader_mux_asset.playback_id
                      ),
                    ]"
                  />
                </figure>
              </div>
              <nuxt-link
                :to="`watch/${video.id}`"
                class="button is-medium is-black is-primary is-light has-text-centered"
              >
                Watch {{ video.mux_video_uploader_mux_asset.title }}
              </nuxt-link>
            </div>
          </div>
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
  methods: {
    createThumbnail(id) {
      return `https://image.mux.com/${id}/thumbnail.png?width=214&height=121&fit_mode=pad`
    },
  },
}
</script>
