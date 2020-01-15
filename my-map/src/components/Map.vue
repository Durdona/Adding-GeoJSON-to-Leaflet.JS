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

          <l-geo-json
            :geojson="geojson"
            :options="options"
            :options-style="styleFunction"
          />

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
import { LMap, LTileLayer, LMarker, LGeoJson } from "vue2-leaflet";
import axios from "axios";
import Forecast from "./Forecast.vue";

export default {
  name: "Map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LGeoJson,
    Forecast
  },
  data() {
    return {
      forecast: [],
      zoom: 3,
      // eslint-disable-next-line no-undef
      center: L.latLng(47.41322, -1.219482),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      // eslint-disable-next-line no-undef
      marker: L.latLng(47.41322, -1.219482),
      enableTooltip: true,
      show: true,
      fillColor: "#e4ce7f",
      geojson: {
        type: "FeatureCollection",
        features: [
          {
            type: "Feature",
            properties: {},
            geometry: {
              type: "LineString",
              coordinates: [
                [-80.87860107421875, 41.091772220976644],
                [-80.5023193359375, 40.45948689837198],
                [-79.77447509765625, 40.31513750307456],
                [-79.63165283203125, 40.60769725157612],
                [-80.53253173828124, 41.22411753058293],
                [-80.8758544921875, 41.10212132036491]
              ]
            }
          },
          {
            type: "Feature",
            properties: {},
            geometry: {
              type: "Point",
              coordinates: [-80.1727294921875, 40.551374198715166]
            }
          }
        ]
      }
    };
  },

  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction
      };
    },
    styleFunction() {
      const fillColor = this.fillColor; // important! need touch fillColor in computed for re-calculate when change fillColor
      return () => {
        return {
          weight: 5,
          color: "#aa09e0",
          opacity: 1,
          fillColor: fillColor,
          fillOpacity: 1
        };
      };
    },
    onEachFeatureFunction() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindTooltip(
          "<div>code:" +
            feature.properties.code +
            "</div><div>nom: " +
            feature.properties.nom +
            "</div>",
          { permanent: false, sticky: true }
        );
      };
    }
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
