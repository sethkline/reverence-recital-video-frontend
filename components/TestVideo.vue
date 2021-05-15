<template>
  <video ref="video" width="100%" height="640" controls></video>
</template>

<script>
import Hls from 'hls.js'

export default {
  props: {
    streamLink: {
      type: String,
    },
  },
  mounted() {
    if (this.streamLink) {
      this.loadVideo()
    }
  },
  methods: {
    loadVideo() {
      const hls = new Hls()
      const stream = this.streamLink
      const video = this.$refs.video
      hls.loadSource(stream)
      hls.attachMedia(video)
      hls.on(Hls.Events.MANIFEST_PARSED, function () {
        video.play()
      })
    },
  },
}
</script>
