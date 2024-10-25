<template>
  <div id="map-container">
    <l-map :zoom="zoom" :center="center" style="height: 100%; width: 100%;">
      <l-tile-layer :url="url" />
      <!-- Ajout du cercle rouge -->
      <l-circle :lat-lng="markerPosition" :radius="radius" color="red" fillOpacity="0.7" fill-color="red" />
      <!-- Ajout du marqueur -->
      <l-marker :lat-lng="markerPosition">
        <l-popup>Voici un marqueur!</l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup, LCircle } from 'vue2-leaflet';
import 'leaflet/dist/leaflet.css';
import axios from 'axios';

export default {
  name: 'MapComponent',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup,
    LCircle
  },
  data() {
    return {
      zoom: 13,
      center: [48.8566, 2.3522],
      url: 'https://server.arcgisonline.com/ArcGIS/rest/services/World_Imagery/MapServer/tile/{z}/{y}/{x}',
      markerPosition: [48.8566, 2.3522],
      radius: 20000
    };
  },
  mounted() {
    this.fetchCoordinates();
  },
  methods: {
    async fetchCoordinates() {
      try {
        const response = await axios.get('http://127.0.0.1:8000/api/coordinates/');
        const { latitude, longitude } = response.data;
        this.center = [latitude, longitude];
        this.markerPosition = [latitude, longitude];
        console.log('Coordonnées récupérées:', response.data);
      } catch (error) {
        console.error('Erreur lors de la récupération des coordonnées:', error);
      }
    }
  }
};
</script>


<style>
#map-container {
  height: 100vh;
  width: 100vw;
}

</style>
