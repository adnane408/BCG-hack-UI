<template>
  <div>
  <nav class="navbar navbar-light bg-light">
    <a class="navbar-brand" href="#">
      <img src="/docs/4.0/assets/brand/bootstrap-solid.svg" width="30" height="30" class="d-inline-block align-top" alt="">
      Bootstrap
    </a>
  </nav>
  <div id="app" style="display: grid; grid-template-columns: 2fr 1fr; height: 100vh;grid-template-rows: 1fr 4fr 1fr">
    <MapComponent ref="mapComponent" style="grid-column: 1/2; grid-row: 2/3;" />
    <div class="city-cards-container" style="grid-column: 2/3; overflow-y: auto; padding: 20px;grid-row: 1/4">
      <h2 style="font-size: 1.5em; margin-bottom: 20px; text-align: center;">Villes du Maroc et Indice de Risque</h2>
      <div
          v-for="city in cities"
          :key="city.name"
          class="city-card"
          @click="focusOnCity(city)"
          style="cursor: pointer; margin-bottom: 10px; border-radius: 8px; padding: 10px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); transition: transform 0.2s;"
          @mouseover="((e) => e.currentTarget.style.transform='scale(1.02')"
          @mouseleave="(e) => e.currentTarget.style.transform='scale(1)'"
      >
        <div class="city-info">
          <h3 class="city-name" style="font-weight: bold;">{{ city.name }}</h3>
          <div class="gauge" style="background-color: #f0f0f0; border-radius: 5px; height: 10px; position: relative;">
            <div :class="['gauge-fill', getGaugeColor(city.gaugeValue)]" :style="{ width: city.gaugeValue + '%' }"></div>
          </div>
        </div>
        <span class="gauge-text" style="font-weight: bold;">{{ city.gaugeValue }}%</span>
      </div>
    </div>
  </div>
  </div>
</template>

<script>
import MapComponent from './components/MapComponent.vue';

export default {
  name: 'App',
  components: {
    MapComponent
  },
  data() {
    return {
      cities: [
        { name: 'Casablanca', gaugeValue: 85, coordinates: [33.5731, -7.5898] },
        { name: 'Rabat', gaugeValue: 65, coordinates: [34.0209, -6.8416] },
        { name: 'Marrakech', gaugeValue: 45, coordinates: [31.6295, -7.9811] },
        { name: 'Fès', gaugeValue: 50, coordinates: [34.0331, -5.0003] },
        { name: 'Tanger', gaugeValue: 75, coordinates: [35.7595, -5.834] },
        { name: 'Agadir', gaugeValue: 60, coordinates: [30.4278, -9.5981] },
        { name: 'Oujda', gaugeValue: 30, coordinates: [34.6826, -1.9042] },
        { name: 'Meknès', gaugeValue: 40, coordinates: [33.8969, -5.5473] },
        { name: 'Tétouan', gaugeValue: 55, coordinates: [35.577, -5.3684] },
        { name: 'Laâyoune', gaugeValue: 20, coordinates: [27.1484, -13.1999] }
      ]
    };
  },
  methods: {
    getGaugeColor(value) {
      if (value > 70) return 'gauge-red';
      else if (value > 40) return 'gauge-orange';
      else return 'gauge-green';
    },
    focusOnCity(city) {
      // Accède à l'instance de la carte dans le composant enfant MapComponent
      this.$refs.mapComponent.centerMap(city.coordinates);
    }
  }
};
</script>

<style>
.city-card {
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s;
}

.city-card:hover {
  transform: scale(1.02);
}

.gauge-fill {
  height: 100%;
  border-radius: 5px;
}

.gauge-red {
  background-color: red;
}

.gauge-orange {
  background-color: orange;
}

.gauge-green {
  background-color: green;
}
</style>
