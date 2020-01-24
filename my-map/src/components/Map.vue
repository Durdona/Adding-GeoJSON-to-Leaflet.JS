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
      <DataList :dataList="dataList" />
      <div id="mapid" class="column is-two-thirds">
        <l-map :zoom="zoom" :center="center" id="l-map">
          <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>

          <l-geo-json
            :geojson="geojson"
            :options="options"
            :options-style="styleFunction"
          />

          <l-circle :lat-lng="circle.center" :radius="circle.radius" />
          <l-marker
            v-for="(dataLists, index) in dataList"
            v-bind:key="index"
            :lat-lng="latLng(dataLists.latitude, dataLists.longitude)"
          ></l-marker>
        </l-map>
      </div>
    </div>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LGeoJson, LCircle } from "vue2-leaflet";
import axios from "axios";
import { latLng } from "leaflet";
import DataList from "./DataList.vue";

export default {
  name: "Map",
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LGeoJson,
    LCircle,
    DataList
  },
  data() {
    return {
      dataList: [],
      zoom: 3,
      // eslint-disable-next-line no-undef
      center: L.latLng(47.41322, -1.219482),
      url: "http://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:
        '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      // eslint-disable-next-line no-undef
      marker: L.latLng(47.41322, -1.219482),
      circle: {
        center: latLng(40.44074158649877, -79.99304294586182),
        radius: 30000
      },
      color: "#ff00ff",
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
                [-80.8978271484375, 40.622291783092706],
                [-80.1068115234375, 41.025499378313754],
                [-79.288330078125, 40.68063802521456],
                [-79.6563720703125, 40.03182061333687],
                [-80.8978271484375, 39.86758762451019],
                [-80.826416015625, 40.44276659332215],
                [-80.88134765625, 40.59727063442024]
              ]
            }
          },
          {
            type: "Feature",
            properties: {},
            geometry: {
              type: "Point",
              coordinates: [-80.00244140625, 40.44694705960048]
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
      this.dataList = r.data;
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
  height: 90vh;
}
#l-map {
  border: 2px solid grey;
}
h1:hover {
  font-weight: 900;
}
</style>
