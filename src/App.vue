<template>
  <div>
    <nav class="navbar navbar-light" style="height: 100px;background-color: #B8001F">
      <a class="navbar-brand" href="#" style="font-size: 2em;">
        <img src="@/assets/1.png" width="50" height="50" class="d-inline-block align-top" alt="">
        Wild Fire Watcher
      </a>
    </nav>

    <!-- New KPI Cards Section -->
    <div class="kpi-cards" style="display: flex; justify-content: space-around; padding: 20px; background-color: #FCFAEE;">
      <div class="kpi-card" style="background-color: white; border-radius: 8px; padding: 20px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); width: 30%; text-align: center;">
        <h3 style="font-size: 1.2em; margin-bottom: 10px;">Total Wildfires</h3>
        <p style="font-size: 1.5em; font-weight: bold;">120</p>
      </div>
      <div class="kpi-card" style="background-color: white; border-radius: 8px; padding: 20px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); width: 30%; text-align: center;">
        <h3 style="font-size: 1.2em; margin-bottom: 10px;">High Risk States</h3>
        <p style="font-size: 1.5em; font-weight: bold;">5</p>
      </div>
      <div class="kpi-card" style="background-color: white; border-radius: 8px; padding: 20px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); width: 30%; text-align: center;">
        <h3 style="font-size: 1.2em; margin-bottom: 10px;">Average Risk Index</h3>
        <p style="font-size: 1.5em; font-weight: bold;">54%</p>
      </div>
    </div>

    <div id="app" style="display: grid; grid-template-columns: 2fr 1fr; height: 100vh; grid-template-rows: 1fr 4fr 1fr">
      <MapComponent ref="mapComponent" style="grid-column: 1/2; grid-row: 2/3; padding-right: 10px" />
      <div class="city-cards-container" style="grid-column: 2/3; overflow-y: auto; padding: 20px; grid-row: 1/4">
        <h2 style="font-size: 1.5em; margin-bottom: 20px; text-align: center;">États des USA et Indice de Risque</h2>
        <div
            v-for="state in states"
            :key="state.name"
            class="city-card"
            @click="focusOnState(state)"
            style="cursor: pointer; margin-bottom: 10px; border-radius: 8px; padding: 10px; box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2); transition: transform 0.2s;"
            @mouseover="((e) => e.currentTarget.style.transform='scale(1.02')"
            @mouseleave="(e) => e.currentTarget.style.transform='scale(1)'"
        >
          <div class="city-info">
            <h3 class="city-name" style="font-weight: bold;font-size: 1.5em;">{{ state.name }}</h3>
            <div class="gauge" style="background-color: #f0f0f0; border-radius: 5px; height: 10px; position: relative;">
              <div :class="['gauge-fill', getGaugeColor(state.gaugeValue)]" :style="{ width: state.gaugeValue + '%' }"></div>
            </div>
          </div>
          <span class="gauge-text" style="font-weight: bold;">{{ state.gaugeValue }}%</span>
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
      states: [
        { name: 'California', gaugeValue: 85, coordinates: [36.7783, -119.4179] },
        { name: 'Texas', gaugeValue: 65, coordinates: [31.9686, -99.9018] },
        { name: 'Florida', gaugeValue: 45, coordinates: [27.7663, -81.6868] },
        { name: 'New York', gaugeValue: 50, coordinates: [40.7128, -74.0060] },
        { name: 'Illinois', gaugeValue: 75, coordinates: [40.6331, -89.3985] },
        { name: 'Pennsylvania', gaugeValue: 60, coordinates: [41.2033, -77.1945] },
        { name: 'Ohio', gaugeValue: 30, coordinates: [40.4173, -82.9071] },
        { name: 'Georgia', gaugeValue: 40, coordinates: [32.1656, -82.9001] },
        { name: 'North Carolina', gaugeValue: 55, coordinates: [35.7822, -80.7935] },
        { name: 'Michigan', gaugeValue: 20, coordinates: [44.3148, -85.6024] }
      ]
    };
  },
  methods: {
    getGaugeColor(value) {
      if (value > 70) return 'gauge-red';
      else if (value > 40) return 'gauge-orange';
      else return 'gauge-green';
    },
    focusOnState(state) {
      this.$refs.mapComponent.centerMap(state.coordinates);
    }
  }
};
</script>

<style>
body {
  background-color: #FCFAEE;
}
.city-card {
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
  transition: transform 0.2s;
  height: 100px;
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
