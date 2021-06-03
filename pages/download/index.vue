<template>
  <div class="container">
    <h1 class="title">Downloads</h1>
    <p>Right Click file name and choose 'save link as' to download</p>
    <table v-if="downloads && downloads.length" class="table">
      <thead>
        <tr>
          <td>Name</td>
          <td>Description</td>
          <td>Size</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="download in downloads" :key="download.id">
          <td>
            <a :href="download.link" download>{{ download.name }}</a>
          </td>
          <td>{{ download.description }}</td>
        </tr>
      </tbody>
    </table>
    <p v-else>There are no downloads right now check back later</p>
  </div>
</template>

<script>
export default {
  middleware: 'auth',
  name: 'Download',
  async asyncData({ $axios }) {
    const { data } = await $axios.get('/downloads')
    data.sort((a, b) =>
      a.name.toLowerCase().localeCompare(b.name.toLowerCase())
    )
    return { downloads: data }
  },
}
</script>
