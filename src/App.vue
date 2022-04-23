<template>
  <v-app>
    <v-app-bar
      app
      color="#CC0100"
      dark
      height="50px"
      style='z-index:2001'
    >
      <v-spacer></v-spacer>

    </v-app-bar>
    <v-main>
      <v-row>
       <!-- <v-col></v-col> -->
       <v-col cols="12" md="10">
        <v-container class="mt-7">
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
                  :fillOpacity="circle.fillOpacity">
                  <l-tooltip
                  :options="{
                    permanent: false}"
                  >
                    <strong>Rank: {{circle.id}}</strong> <br> Total Visits: {{circle.visits}}
                  </l-tooltip>
                </l-circle-marker>
              </l-map>
            </v-card>
        </v-container>
       </v-col>
      <v-col class="mx-auto">
        <v-card class="pa-5 mt-10 mr-5">
          <div class='my-legend'>
            <div class='legend-title'>Population Density</div>
            <div class='legend-scale'>
              <ul class='legend-labels'>
                <li><span style='background:#edf8fb;'></span>1-1,500</li>
                <li><span style='background:#bfd3e6;'></span>1,501-3,500</li>
                <li><span style='background:#9ebcda;'></span>3,501-7,000</li>
                <li><span style='background:#8c96c6;'></span>7,0001-12,000</li>
                <li><span style='background:#8856a7;'></span>12,001-25,000</li>
                <li><span style='background:#810f7c;'></span>>25,000</li>
              </ul>
            </div>
            <div class='legend-source'>Source: <a href="#link to source">Name of source</a></div>   
          </div>    
          <!-- <div class="text-center dot1 mt-10"></div>
          <div class="mx-auto"> Less Visits</div>
          <v-spacer></v-spacer>
          <div class="vl ml-14 mb-5 mt-5"></div>
          <div class="text-center dot2 ml-6"></div>
          <div>More Visits</div> -->
        </v-card>
        <!-- <v-spacer></v-spacer>
      <div class="text-center dot1 mx-auto"></div>
      <div class="mx-auto"> Less Visits</div>
      <v-spacer></v-spacer>
      <div class="vl ml-14 mb-5 mt-5"></div>
      <div class="text-center dot2 ml-6"></div>
      <div>More Visits</div> -->
      </v-col>
      </v-row>
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
import { LMap, LTileLayer, LGeoJson, LControlScale, LCircleMarker, LTooltip} from "vue2-leaflet";
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
    LCircleMarker,
    LTooltip
  },


  data() {

    return {
      map: true,
      enableTooltip: true,
      zoom: 9,
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
          radius: 2036124/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 2036124
        },
        { id: '2',
          center: [47.2377519755425, -122.480298527615],
          radius: 1994079/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1994079
        },
        { id: '3',
          center: [47.7590355176595, -122.154068022324],
          radius: 1755491/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1755491
        },
        { id: '4',
          center: [47.1599769333971, -122.295223147819],
          radius: 1548847/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1548847
        },
        { id: '5',
          center: [47.5417638713567, -122.050644059283],
          radius: 1529826/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1529826
        },
        { id: '6',
          center: [47.1128888508873, -122.291666210924],
          radius: 1527335/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1527335
        },
        { id: '7',
          center: [47.4962571449308, -122.199851723218],
          radius: 1485331/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1485331
        },
        { id: '8',
          center: [47.3130694995818, -122.305231085636],
          radius: 1469152/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1469152
        },
        { id: '9',
          center: [47.4551854083819, -122.258933363672],
          radius: 1461693/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1461693
        },
        { id: '10',
          center: [47.6725464028309, -122.103446825318],
          radius: 1410518/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1410518
        },
        { id: '11',
          center: [47.1631128985045, -122.511436143694],
          radius: 1410272/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1410272
        },
        { id: '12',
          center: [47.9104543526083, -122.227315450308],
          radius: 1370193/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1370193
        },
        { id: '13',
          center: [47.3660073070965, -122.203416396167],
          radius: 1305197/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1305197
        },
        { id: '14',
          center: [48.1489489002533, -122.19274811851],
          radius: 1285735/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1285735
        },
        { id: '15',
          center: [47.3613164771609, -122.607974698474],
          radius: 1278391/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1278391
        },
        { id: '16',
          center: [47.9991060530233, -122.101281006088],
          radius: 1252088/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1252088
        },
        { id: '17',
          center: [47.5737553851301, -122.173513510975],
          radius: 1185260/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1185260
        },
        { id: '18',
          center: [47.1725553918957, -122.172303032396],
          radius: 1136295/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1136295
        },
        { id: '19',
          center: [47.5222520295411, -122.368754872501],
          radius: 1050824/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 1050824
        },
        { id: '20',
          center: [47.6086499037094, -122.338778749914],
          radius: 997170/50000,
          color: '#CC0100',
          fillColor: '#CC0100',
          fillOpacity: .6,
          visits: 997170
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
          opacity: .7,
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
            opacity: .7
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
    // markerOptions() {
    //   return (feature) => {
    //     feature.bindTooltip(
    //       "<div> Hello"
    //     )
    //   }
    // }
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

.dot1 {
  height: 25px;
  width: 25px;
  border: 2px solid #CC0100;
  fill-opacity: .6;
  filter: alpha(opacity=100);
  background-color: rgb(220, 164, 164);
  border-radius: 50%;
  display: inline-block;
}
.dot2 {
  height: 75px;
  width: 75px;
  border: 2px solid #CC0100;
  fill-opacity: .6;
  filter: alpha(opacity=100);
  background-color: rgb(220, 164, 164);
  border-radius: 50%;
  display: inline-block;
}

.vl {
  border-left: 6px solid rgba(91, 89, 89, 0.741);
  height: 200px;
}

  .my-legend .legend-title {
    text-align: left;
    margin-bottom: 10px;
    font-weight: bold;
    font-size: 90%;
    }
  .my-legend .legend-scale ul {
    margin: 0;
    padding: 0;
    float: left;
    list-style: none;
    }
  .my-legend .legend-scale ul li {
    display: block;
    float: left;
    width: 150px;
    margin-bottom: 6px;
    text-align: center;
    font-size: 75%;
    list-style: none;
    }
  .my-legend ul.legend-labels li span {
    display: block;
    float: left;
    height: 20px;
    width: 50px;
    }
  .my-legend .legend-source {
    font-size: 60%;
    color: #999;
    clear: both;
    }
  .my-legend a {
    color: #777;
    }
</style>
