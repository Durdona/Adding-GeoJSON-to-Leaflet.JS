<template>
  <div class="section">
    <div class="columns">
      <div class="column has-background-black-ter"></div>
      <div class="column has-background-grey-darker"></div>
      <div class="column has-background-grey-dark"></div>
      <div class="column has-background-grey-darker"></div>
      <div class="column has-background-black-ter"></div>
    </div>
    <div>
      <h1
        class="title is-1 has-text-centered is-size-desktop has-text-black-bis "
      >
        Interactive Map
      </h1>
    </div>
    <div class="columns">
      <Forecast :forecast="forecast" />
      <div id="mapid" class="column is-two-thirds">
        <l-map :zoom="zoom" :center="center">
          <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
          <l-marker
            v-for="(forecasts, index) in forecast"
            v-bind:key="index"
            :lat-lng="latLng(forecasts.latitude, forecasts.longitude)"
          ></l-marker>
        </l-map>
      </div>
    </div>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker } from "vue2-leaflet";
import axios from "axios";
import Forecast from "./Forecast.vue";

export default {
  name: "Map",
  data() {
    return {
      forecast: [],
      zoom: 2,
      // eslint-disable-next-line no-undef
      center: L.latLng(47.41322, -1.219482),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      // eslint-disable-next-line no-undef
      marker: L.latLng(47.41322, -1.219482)
    };
  },
  mounted: function() {
    axios.get(" https://api.openbrewerydb.org/breweries").then(r => {
      this.forecast = r.data;
    });
  },
  methods: {
    latLng: function(lat, lng) {
      // eslint-disable-next-line no-undef
      return L.latLng(lat, lng);
    }
  },
  props: {
    msg: String
  },
  components: {
    LMap,
    LTileLayer,
    LMarker,
    Forecast
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.section {
  padding: 1rem 0.5rem;
}
h1 {
  margin-bottom: 1rem;
}
#mapid {
  height: 83vh;
}
</style>
