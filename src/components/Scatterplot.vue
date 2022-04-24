<template>
  <div class="line-wrapper">
    <v-chart :options="line" autoresize />
  </div>
</template>

<script>
import ECharts from "vue-echarts";
import "echarts/lib/chart/scatter";
import "echarts/lib/chart/effectScatter"
import 'echarts/lib/component/legend'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/title'
import 'echarts/lib/component/grid'
export default {
  components: {
      "v-chart": ECharts,
  },
    data: () => ({
        line: {
            xAxis: {
                name: 'Total Visitors',
                scale: true,
                splitLine: {
                    lineStyle: {
                        type: 'dashed'
                    }
                }
            },
            yAxis: {
                name: 'Population Density',
                scale: true,
                    splitLine: {
                    lineStyle: {
                        type: 'dashed'
                    }
                }
            },
            width: '80%',
            height: '80%',
            series: [
                { type: 'effectScatter',
                name: 'Target Venue Outlier',
                symbolSize: function (data) {
                    return (data[0]/80000);
                },
                color: "#B71C1C",
                label: {
                        show: true,
                        formatter: function (param) {
                        return param.data[2];
                        },
                            minMargin: 10,
                            position: 'top'
                        },
                data: [
                    [997170, 25986, "20", "1401 2nd Ave, Seattle, WA, United States"],
                    [1050824, 7722, "19", "2800 SW Barton St, Seattle, WA, United States"],
                ]
                },
                {   
                    symbolSize: function (data) {
                                    return (data[0]/50000);
                                },
                    data: [
                            [1136295, 1351, "18", "9400 192nd Ave E, Bonney Lake, WA, United States"],
                            [1185260, 3536, "17", "4053 Factoria Square Mall SE, Bellevue, WA, United States"],
                            [1252088, 1151, "16", "9601 Market Pl, Lake Stevens, WA, United States"],
                            [1278391, 897, "15", "11400 51st Ave NW, Gig Harbor, WA, United States"],
                            [1285735, 625, "14", "16818 Twin Lakes Ave, Marysville, WA, United States"],
                            [1305197, 5425, "13", "26301 104th Ave SE, Kent, WA, United States"],
                            [1370193, 3597, "12", "405 SE Everett Mall Way, Everett, WA, United States"],
                            [1410272, 3413, "11", "5618 Lakewood Town Center Blvd SW, Lakewood, WA, United States"],
                            [1410518, 3492, "10", "17700 NE 76th St, Redmond, WA, United States"],
                            [1461693, 3343, "9", "301 Strander Blvd, Tukwila, WA, United States"],
                            [1469152, 4259, "8", "2201 S Commons, Federal Way, WA, United States"],
                            [1485331, [(2101 + 4745)/2], "7", "1215 N Landing Way, Renton, WA, United States"],
                            [1527335, [(2624 + 3288)/2], "6", "10302 156th St E, Puyallup, WA, United States"],
                            [1529826, [(555 + 3267)/2], "5", "755 NW Gilman Blvd, Issaquah, WA, United States"],
                            [1548847, [(2800 + 2624)/2], "4", "3310 S Meridian, Puyallup, WA, United States"],
                            [1755491, [(1321+4125)/2], "3", "13950 NE 178th Pl, Woodinville, WA, United States"],
                            [1994079, [(5720+3646)/2], "2", "3320 S 23rd St, Tacoma, WA, United States"],
                            [2036124, [(5293+4208)/2], "1", "18305 Alderwood Mall Pkwy, Lynnwood, WA, United States"]
                    ],
                    type: 'scatter',
                    name: 'Target Venue',
                    label: {
                        show: true,
                        formatter: function (param) {
                        return param.data[2];
                        },
                            minMargin: 10,
                            position: 'top'
                        },
                    color: '#CC0100'
                }
            ],
            tooltip: {
                trigger: 'item',    
                formatter: function (param) {
                return "<strong>Location: </strong>" + param.data[3] + "<br>" + "<strong>Visitors:</strong> " + param.data[0].toLocaleString() + "<br>" + "<strong>Population Density: </strong>" + param.data[1].toLocaleString();
                }
            },
            legend: {
            data: ['Target Venue'],
            left: 'center',
  },
        }
    })
}
</script>

<style scoped>
.echarts {
    width: 100%;
    height: 400px;
}
</style>