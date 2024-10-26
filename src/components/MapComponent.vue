<template>
  <div id="map-container">
    <l-map
        ref="mapRef"
        :zoom="zoom"
        :center="center"
        style="height: 100%; width: 100%; border-radius: 15px; overflow: hidden; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);"
        @load="onMapLoad"
    >
      <l-tile-layer :url="url" />
      <l-control-scale position="bottomleft" />
    </l-map>
  </div>
</template>

<script>
import {LMap, LTileLayer, LControlScale} from 'vue2-leaflet';
import 'leaflet/dist/leaflet.css';
import L from 'leaflet';
import axios from 'axios';

export default {
  name: 'MapComponent',
  components: {
    LMap,
    LTileLayer,
    LControlScale
  },
  data() {
    return {
      zoom: 6,
      center: [37.0902, -95.7129], // Coordonnées centrales pour l'Amérique (environ les États-Unis)
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      markers: [
        {
          position: [34.0522, -118.2437], // Los Angeles
          popupMessage: 'Los Angeles',
          radius: 2000000
        },
        {
          position: [40.7128, -74.0060], // New York
          popupMessage: 'New York',
          radius: 300000
        },
        {
          position: [25.7617, -80.1918], // Miami
          popupMessage: 'Miami',
          radius: 1500
        }
      ]
    };
  },
  mounted() {
    console.log('Markers array:', this.markers); // Log markers before rendering them
    this.fetchCoordinates();
  },
  methods: {
    async fetchCoordinates() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/coordinates/');
        const {latitude, longitude} = response.data;

        // Add new marker with fetched coordinates
        this.markers.push({
          position: [latitude, longitude],
          popupMessage: 'Fetched Marker',
          radius: 2500 // Specify radius for the circle
        });

        // Log updated markers
        console.log('Coordonnées récupérées:', response.data);
        console.log('Updated Markers array:', this.markers);

        // Redraw markers after updating data
        this.drawMarkers();
      } catch (error) {
        console.error('Erreur lors de la récupération des coordonnées:', error);
      }
    },
    onMapLoad() {
      this.drawMarkers();
      this.disableZoom();
    },
    drawMarkers() {
      const map = this.$refs.mapRef.mapObject;
      if (map) {
        this.markers.forEach(marker => {
          // Add the marker
          const leafletMarker = L.marker(marker.position).addTo(map);
          leafletMarker.bindPopup(marker.popupMessage);

          // Add a circle around the marker
          L.circle(marker.position, {
            color: 'red',
            fillColor: '#f03',
            fillOpacity: 0.5,
            radius: marker.radius // Use the radius specified in the marker data
          }).addTo(map);
        });
      }
    },
    disableZoom() {
      const map = this.$refs.mapRef.mapObject;
      if (map) {
        map.scrollWheelZoom.disable();
        map.doubleClickZoom.disable();
        map.zoomControl.remove();
      } else {
        console.error('La carte n\'est pas encore initialisée');
      }
    }
  }
};
</script>

<style>
#map-container {
  height: 100%;
  width: 100%;
  margin: 20px;
  border-radius: 15px;
}
</style>
