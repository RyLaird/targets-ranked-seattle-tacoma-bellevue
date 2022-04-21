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
      zoom: 10,
      center: [47.60665052929262, -122.33503601679615],
      geojson_popden: null,
      url: 'https://api.mapbox.com/styles/v1/rlaird2/cl29hg7ah000914my0rv48xwe/tiles/256/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoicmxhaXJkMiIsImEiOiJja2JmN2x6aWIwc3VmMzVvNDl5Mzk1ejNuIn0.rrNaMaCy39_ntp7qPvp0dQ',
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
      return (feature) => {
        return {
          weight: .5,
          color: "white",
          opacity: .7,
          dashArray: '3',
          fillColor: this.getColor(feature.properties.popDensity),
          fillOpacity: .6
        };
      };
    },
    onEachFeatureFunction1() {
      if (!this.enableTooltip) {
        return () => {};
      }
      return (feature, layer) => {
        layer.bindTooltip(
          "<div>Population Density: " +
            feature.properties.popDensity +
          "</div>",
          { permanent: false, sticky: true }
        ),
        layer.on('mouseover', function (e){
          e.target.setStyle({
            weight:4,
            dashArray: '',
          })
        });
        layer.on('mouseout', function (e){
          e.target.setStyle({
            weight: .5,
            dashArray: '6',
            opacity: .7
          })
        });
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
    getColor(d) {
      return d >  25000 ? '#810f7c' :
      d > 12000 ? '#8856a7':
      d > 7000 ? '#8c96c6':
      d > 3500 ? '#9ebcda':
      d > 1500 ? '#bfd3e6':
      '#edf8fb';
    },
  }
};
</script>
