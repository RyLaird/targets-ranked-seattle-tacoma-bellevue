<template>
  <v-app>
    <v-app-bar
      app
      color="#CC0100"
      dark
    >
      <v-spacer></v-spacer>

    </v-app-bar>

    <v-main>
      <v-container>
          <v-card elevation-10>
            <l-map ref="Map"
              :zoom="zoom"
              :center="center"
              style="height: 720px"
            >
              <!-- sourcing -->
              <l-tile-layer
                :url="url"
                :attribution="attribution"
              />
              <l-control-scale position="topright" :imperial="true" :metric="false"></l-control-scale>
              <!-- pop density geojson -->
              <l-geo-json
                :geojson="geojson_popden"
                :options="options"
                :options-style="styleFunction_popden"
                @click="zoomToFeature()"
              />
              <l-geo-json
              :geojson="geojson_targets"
              :options="markerOptions"
              />
            </l-map>
          </v-card>
      </v-container>
      <div>
        <v-card class="mt-5 mb-8">
          <v-card-text>
            <v-container>
              <v-row>
                <v-col>
                <h1 class='text-center mb-5'>
                  Annual Target Foot Traffic (2021)
                  </h1>
                  <chart></chart>
                  </v-col>
              </v-row>
            </v-container>
          </v-card-text>
        </v-card>
        </div>
    </v-main>
  </v-app>
</template>

<script>
// import Map from './components/Map';
import { LMap, LTileLayer, LGeoJson, LControlScale} from "vue2-leaflet";
import 'leaflet/dist/leaflet.css'
import {Icon} from 'leaflet';
import Chart from '/Users/rlaird/Documents/targets-ranked-seattle-tacoma-bellevue/src/components/Chart.vue'



delete Icon.Default.prototype._getIconUrl;
Icon.Default.mergeOptions({
  iconRetinaUrl: require('/src/assets/target_logo.png'),
  iconUrl: require('/src/assets/target_logo.png'),
  // iconAnchor: [0,0],
  shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
  shadowSize: [30,30],
  iconSize: [50,50]
});

export default {
  name: 'App',

  components: {
    LMap,
    LTileLayer,
    Chart,
    LGeoJson,
    LControlScale,
  },


  data() {
    return {
      map: true,
      enableTooltip: true,
      zoom: 10,
      center: [47.60665052929262, -122.33503601679615],
      geojson_popden: null,
      geojson_targets: null,
      url: 'https://api.mapbox.com/styles/v1/rlaird2/cl29hg7ah000914my0rv48xwe/tiles/256/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoicmxhaXJkMiIsImEiOiJja2JmN2x6aWIwc3VmMzVvNDl5Mzk1ejNuIn0.rrNaMaCy39_ntp7qPvp0dQ',
      attribution:
      '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions>CARTO</a>',
    }
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction
      } 
    },
    markerOptions() {
      return {
        onEachFeature: this.onEachMarkerFunction
      }
    },
    styleFunction_popden() {
      return (feature) => {
        return {
          weight: 1,
          color: "white",
          opacity: .5,
          dashArray: '',
          fillColor: this.getColor(feature.properties.popDensity),
          fillOpacity: .4
        };
      };

    },
    onEachFeatureFunction() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindPopup(
          "<div>Population Density: " +
            Math.round(feature.properties.popDensity) +
          "</div>",
          { permanent: false, sticky: true }
        ),
        layer.on('mouseover', function (e){
          e.target.setStyle({
            weight:6,
            color: 'white',
            opacity: .5
          })
        });
        layer.on('mouseout', function (e){
          e.target.setStyle({
            weight: 1,
            color: "white",
            opacity: .5
          })
        });
      };
    },
    onEachMarkerFunction() {
      return (feature, layer) => {
        var props = feature.properties
        layer.bindPopup(
          "<div>Visitors: " +
          Math.round(props["Total Visits"])+
          "</div>",
          { permanent: false, sticky: false}

        ),
        layer.bindTooltip(
          "<div> " +
          props["Rank"] +
          "</div>",
          {permanent: true, sticky: false, className: "label"}
        )
      }
    }
  },
  async created() {
    this.loading = true;

    const response_popden = await fetch("https://raw.githack.com/RyLaird/targets-ranked-seattle-tacoma-bellevue/master/src/data/Seattle_pop_zipCode.json?token=GHSAT0AAAAAABTXKTYN4MQA772OJ7LFVBSCYTAQELQ")
    const data_popden = await response_popden.json();
    this.geojson_popden = data_popden;

    const response_targets = await fetch("https://raw.githubusercontent.com/RyLaird/targets-ranked-seattle-tacoma-bellevue/master/src/data/target_locations.geojson")
    const data_targets = await response_targets.json();
    this.geojson_targets = data_targets;
    
    this.loading = false;
  },
  methods: {
    recenterMap() {
      this.$refs.Map.mapObject.flyTo([39, -105], 6)
    },
    zoomToFeature(e) {
      this.$refs.Map.mapObject.fitBounds(e.target.getBounds())
    },
    getColor(d) {
      return d >  25000 ? '#810f7c' :
      d > 12000 ? '#8856a7':
      d > 7000 ? '#8c96c6':
      d > 3500 ? '#9ebcda':
      d > 1500 ? '#bfd3e6':
      '#edf8fb';
    },
    getSize(d) {
      return d > 2000000 ? '[100,100]' :
      '[50,50]';
    }
  }
};
</script>

<style scoped>
.leaflet-tooltip {
  background-color: transparent;
  border: transparent;
  box-shadow: none;
  }
</style>
