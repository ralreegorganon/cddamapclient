<template>
  <div id="app" class="map-fill">
    <Map />
  </div>
</template>

<script>
import Map from './components/Map'

export default {
  name: 'app',
  components: {
    Map
  }
}
</script>

<style>
body {
  padding: 0;
  margin: 0;
}

html,
body,
.map-fill {
  height: 100%;
}
</style>
