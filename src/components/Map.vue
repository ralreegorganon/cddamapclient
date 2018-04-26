<template>
  <div id="map" class="map"></div>
</template>

<script type="text/javascript">
import axios from 'axios'

export default {
  data () {
    return {
      maxNativeZoom: null,
      map: null,
      tileLayer: null,
      selectedCells: null,
      currentLayerUrl: null,
      layers: []
    }
  },
  mounted () {
    this.initMap()
    this.initLayers()
    this.initMapEvents()
  },
  methods: {
    initMap () {
      this.map = L.map('map', {
        crs: L.CRS.Simple
      }).setView([-128, 128], 1)

      this.selectedCells = L.geoJSON(null, {
        coordsToLatLng: nc => {
          return this.map.unproject([nc[0], nc[1]], this.maxNativeZoom)
        }
      }).bindTooltip(layer => {
        return layer.feature.properties.name
      }).addTo(this.map)

      axios.get('https://cddamapapi.ralreegorganon.com/api/worlds/1').then(
        response => {
          console.log(response)

          this.maxNativeZoom = response.data.maxz
          this.currentLayerUrl = `https://cddamapapi.ralreegorganon.com/api/worlds/${response.data.id}/layers/${response.data.z['10'].layerId}`
          this.tileLayer = L.tileLayer(`${this.currentLayerUrl}/tiles/{z}/{x}/{y}.png`, {
            maxNativeZoom: this.maxNativeZoom,
            attribution: '',
            noWrap: true,
            crs: L.CRS.Simple
          })
          this.tileLayer.addTo(this.map)
        },
        error => {
          console.log(error)
        }
      )
    },
    initLayers () {},
    initMapEvents () {
      this.map.on('click', e => {
        console.log(this.maxNativeZoom)
        const p = this.map.project(e.latlng, this.maxNativeZoom)
        axios.get(`${this.currentLayerUrl}/cells/${p.x}/${p.y}`).then(
          response => {
            console.log(response)
            this.selectedCells.clearLayers()
            this.selectedCells.addData(response.data)
          }, error => {
            console.log(error)
          })
      })
    }
  }
}
</script>

<style>
.map {
  height: 100%;
  width: 100%;
}
</style>
