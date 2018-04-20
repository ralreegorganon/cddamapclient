<template>
  <div id="map" class="map"></div>
</template>

<script type="text/javascript">
export default {
  data () {
    return {
      maxNativeZoom: null,
      map: null,
      tileLayer: null,
      layers: []
    }
  },
  mounted () {
    this.initMap()
    this.initLayers()
  },
  methods: {
    initMap () {
      this.maxNativeZoom = 5

      this.map = L.map('map', {
        crs: L.CRS.Simple
      }).setView([-128, 128], 1)

      this.tileLayer = L.tileLayer('http://localhost:8002/api/worlds/1/layers/3/tiles/{z}/{x}/{y}.png', {
        maxZoom: 10,
        maxNativeZoom: this.maxNativeZoom,
        attribution: '',
        noWrap: true,
        crs: L.CRS.Simple
      })

      this.tileLayer.addTo(this.map)
    },
    initLayers () {}
  }
}
</script>

<style>
.map {
  height: 100%;
  width: 100%;
}
</style>
