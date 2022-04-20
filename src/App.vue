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
            <l-map ref="myMap"
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
              />
            </l-map>
          </v-card>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
// import Map from './components/Map';
import { LMap, LTileLayer, LGeoJson, LControlScale} from "vue2-leaflet";
import 'leaflet/dist/leaflet.css'

export default {
  name: 'App',

  components: {
    LMap,
    LTileLayer,
    LGeoJson,
    LControlScale
  },

  data() {
    return {
      map: true,
      enableTooltip: true,
      zoom: 6,
      center: [47.60665052929262, -122.33503601679615],
      geojson_popden: null,
      url: 'https://api.mapbox.com/styles/v1/rlaird2/ckwo8jew23mns14lrzvnnxh63/tiles/256/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoicmxhaXJkMiIsImEiOiJja2JmN2x6aWIwc3VmMzVvNDl5Mzk1ejNuIn0.rrNaMaCy39_ntp7qPvp0dQ',
       attribution:
      '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions>CARTO</a>',
    }
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction1
      }
    },
    styleFunction_popden() {
      return () => {
        return {
          weight: 1,
          color: "#ECEFF1",
          opacity: 1,
          fillColor: "#7E57C2",
          fillOpacity: 1
        };
      };
    },
    onEachFeatureFunction1() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindPopup(
          "<div>Name: " +
            feature.properties.HA_NAME +
            "</div><div>Estimated Horses: " +
            feature.properties.EST_HORSE_POP +
            "</div><div> Estimated Burros: " +
            feature.properties.EST_BURRO_POP +
          "</div>",
          { permanent: false, sticky: true }
        );
      };
    },
  },
  async created() {
    this.loading = true;
    const response_popden = await fetch("https://raw.githack.com/RyLaird/targets-ranked-seattle-tacoma-bellevue/master/src/data/Seattle_pop_zipCode.json?token=GHSAT0AAAAAABTXKTYN4MQA772OJ7LFVBSCYTAQELQ")
    const data_popden = await response_popden.json();
    this.geojson_popden = data_popden;
    this.loading = false;
  },
  methods: {
    recenterMap() {
      this.$refs.myMap.mapObject.flyTo([39, -105], 6)
    },
  }
};
</script>
