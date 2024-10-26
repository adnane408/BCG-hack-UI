<template>
  <div id="map-container">
    <l-map ref="mapRef" :zoom="zoom" :center="center" style="height: 100%; width: 100%; border-radius: 15px; overflow: hidden; box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);">
      <l-tile-layer :url="url" />
      <l-circle :lat-lng="markerPosition" :radius="radius" color="red" :fill-opacity="0.7" :fill-color="'red'" />
      <l-marker :lat-lng="markerPosition">
        <l-popup>Voici un marqueur!</l-popup>
      </l-marker>
      <l-control-scale position="bottomleft" /> <!-- Ajout de l'échelle -->

    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup, LCircle,LControlScale } from 'vue2-leaflet';
import 'leaflet/dist/leaflet.css';
import axios from 'axios';

export default {
  name: 'MapComponent',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LCircle,
    LControlScale
  },
  data() {
    return {
      zoom: 6,
      center: [31.7917, -7.0926],
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      markerPosition: [31.7917, -7.0926],
      radius: 2000
    };
  },
  mounted() {
    this.fetchCoordinates();
  },
  methods: {
    async fetchCoordinates() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/coordinates/');
        const {latitude, longitude} = response.data;
        this.center = [latitude, longitude];
        this.markerPosition = [latitude, longitude];
        console.log('Coordonnées récupérées:', response.data);
      } catch (error) {
        console.error('Erreur lors de la récupération des coordonnées:', error);
      }
    },
    centerMap(coordinates) {
      const map = this.$refs.mapRef.mapObject; // Utiliser mapObject pour obtenir l'instance de la carte
      if (map) {
        map.setView(coordinates, 10); // Utiliser setView sur l'instance de la carte
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
  border-radius: 15px; /* Ajusté pour une meilleure apparence */
}
</style>
