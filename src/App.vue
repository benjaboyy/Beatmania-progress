<template>
  <div id="app">
    <beatmania-app></beatmania-app>
  </div>
</template>

<script>
import BeatmaniaApp from './assets/pages/SongList.vue'

export default {
  host: "http://localhost:3000",
  name: 'App',
  components: {
    BeatmaniaApp
  }
}
</script>
