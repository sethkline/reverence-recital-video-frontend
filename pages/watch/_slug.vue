<template>
  <div>
    <a class="button button-primary m-3" @click="$router.go(-1)">
      <span icon="arrow-left"></span> go back</a
    >
    <div class="container is-fullhd">
      <test-video :stream-link="playbackLink" />
    </div>
  </div>
</template>

<script>
export default {
  middleware: 'auth',
  async asyncData({ params, $axios }) {
    const { data } = await $axios.get(`/videos/${params.slug}`)
    return { video: data }
  },
  computed: {
    playbackLink() {
      const videoId = this.video.mux_video_uploader_mux_asset.playback_id
      return `https://stream.mux.com/${videoId}.m3u8`
    },
  },
}
</script>
