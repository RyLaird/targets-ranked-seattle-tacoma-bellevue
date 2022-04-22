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
              <!-- <l-geo-json
                :geojson="geojson_targets"
                :options="markerOptions"
                :options-style="styleFunction_marker"
              /> -->
              <l-circle-marker v-for="circle in circles"
                :key="circle.id"
                :lat-lng="circle.center"
                :radius="circle.radius"
                :color="circle.color"
                :fillColor="circle.fillColor"
                :fillOpacity="circle.fillOpacity"
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
import { LMap, LTileLayer, LGeoJson, LControlScale, LCircleMarker} from "vue2-leaflet";
import 'leaflet/dist/leaflet.css'
// import {Icon} from 'leaflet';
import Chart from '/Users/rlaird/Documents/targets-ranked-seattle-tacoma-bellevue/src/components/Chart.vue'


// const iconSize = [60,60]
// delete Icon?.Default.prototype._getIconUrl;
// Icon.Default.mergeOptions({
//   iconRetinaUrl: require('/src/assets/target_logo.png'),
//   iconUrl: require('/src/assets/target_logo.png'),
//   // iconAnchor: [0,0],
//   shadowUrl: require('leaflet/dist/images/marker-shadow.png'),
//   shadowSize: [30,30],
//   iconSize: iconSize
// });



export default {
  name: 'App',

  components: {
    LMap,
    LTileLayer,
    Chart,
    LGeoJson,
    LControlScale,
    LCircleMarker
  },


  data() {

    return {
      map: true,
      enableTooltip: true,
      zoom: 10,
      center: [47.60665052929262, -122.33503601679615],
      geojson_popden: null,
      // geojson_targets: null,
      url: 'https://api.mapbox.com/styles/v1/rlaird2/cl29hg7ah000914my0rv48xwe/tiles/256/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoicmxhaXJkMiIsImEiOiJja2JmN2x6aWIwc3VmMzVvNDl5Mzk1ejNuIn0.rrNaMaCy39_ntp7qPvp0dQ',
      attribution:
      '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors &copy; <a href="https://carto.com/attributions>CARTO</a>',

      // circle1: {
      //   center: [47.8325549090366, -122.265984791228],
      //   radius: 30,
      //   color: '#CC0100',
      //   fillColor: '#CC0100',
      //   fillOpacity: .6
      // },
      circles: [ { id: '1',
          center: [47.8325549090366, -122.265984791228],
          radius: 2036124/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '2',
          center: [47.2377519755425, -122.480298527615],
          radius: 1994079/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '3',
          center: [47.7590355176595, -122.154068022324],
          radius: 1755491/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '4',
          center: [47.1599769333971, -122.295223147819],
          radius: 1548847/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '5',
          center: [47.5417638713567, -122.050644059283],
          radius: 1529826/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '6',
          center: [47.1128888508873, -122.291666210924],
          radius: 1527335/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '7',
          center: [47.4962571449308, -122.199851723218],
          radius: 1485331/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '8',
          center: [47.3130694995818, -122.305231085636],
          radius: 1469152/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '9',
          center: [47.4551854083819, -122.258933363672],
          radius: 1461693/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '10',
          center: [47.6725464028309, -122.103446825318],
          radius: 1410518/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '11',
          center: [47.1631128985045, -122.511436143694],
          radius: 1410272/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '12',
          center: [47.9104543526083, -122.227315450308],
          radius: 1370193/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '13',
          center: [47.3660073070965, -122.203416396167],
          radius: 1305197/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '14',
          center: [48.1489489002533, -122.19274811851],
          radius: 1285735/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '15',
          center: [47.3613164771609, -122.607974698474],
          radius: 1278391/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '16',
          center: [47.9991060530233, -122.101281006088],
          radius: 1252088/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '17',
          center: [47.5737553851301, -122.173513510975],
          radius: 1185260/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '18',
          center: [47.1725553918957, -122.172303032396],
          radius: 1136295/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '19',
          center: [47.5222520295411, -122.368754872501],
          radius: 1050824/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        { id: '20',
          center: [47.6086499037094, -122.338778749914],
          radius: 997170/45000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6
        },
        ]
    }
  },
  computed: {
    options() {
      return {
        onEachFeature: this.onEachFeatureFunction
      } 
    },
    // markerOptions() {
    //   return {
    //     onEachFeature: this.onEachMarkerFunction
    //   }
    // },
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
    // styleFunction_marker() {
    //   return (feature) => {
    //     return {
    //     iconSize: this.getSize(feature.properties.Rank)
    //   };
    // }
    // },
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
    // onEachMarkerFunction() {
    //   return (feature, layer) => {
    //     var props = feature.properties
    //     layer.bindPopup(
    //       "<div>Visitors: " +
    //       Math.round(props["Total Visits"])+
    //       "</div>",
    //       { permanent: false, sticky: false}

    //     ),
    //     layer.bindTooltip(
    //       "<div> " +
    //       props["Rank"] +
    //       "</div>",
    //       {permanent: true, sticky: false, className: "label"}
    //     )
    //   }
    // },
  },
  async created() {
    this.loading = true;

    const response_popden = await fetch("https://raw.githack.com/RyLaird/targets-ranked-seattle-tacoma-bellevue/master/src/data/Seattle_pop_zipCode.json?token=GHSAT0AAAAAABTXKTYN4MQA772OJ7LFVBSCYTAQELQ")
    const data_popden = await response_popden.json();
    this.geojson_popden = data_popden;

    // const response_targets = await fetch("https://raw.githubusercontent.com/RyLaird/targets-ranked-seattle-tacoma-bellevue/master/src/data/target_locations.geojson")
    // const data_targets = await response_targets.json();
    // this.geojson_targets = data_targets;
    
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
    // getSize(d) {
    //   return d > 10 ? [200,200]:
    //   [30,30];
    // }
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
