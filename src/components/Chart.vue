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
                name: 'Target Store Outlier',
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
                    [997170, 25986, "20"],
                    [1050824, 7722, "19"],
                ]
                },
                {   
                    symbolSize: function (data) {
                                    return (data[0]/50000);
                                },
                    data: [
                            [1136295, 1351, "18"],
                            [1185260, 3536, "17"],
                            [1252088, 1151, "16"],
                            [1278391, 897, "15"],
                            [1285735, 625, "14"],
                            [1305197, 5425, "13"],
                            [1370193, 3597, "12"],
                            [1410272, 3413, "11"],
                            [1410518, 3492, "10"],
                            [1461693, 3343, "9"],
                            [1469152, 4259, "8"],
                            [1485331, [(2101 + 4745)/2], "7"],
                            [1527335, [(2624 + 3288)/2], "6"],
                            [1529826, [(555 + 3267)/2], "5"],
                            [1548847, [(2800 + 2624)/2], "4"],
                            [1755491, [(1321+4125)/2], "3"],
                            [1994079, [(5720+3646)/2], "2"],
                            [2036124, [(5293+4208)/2], "1"]
                    ],
                    type: 'scatter',
                    name: 'Target Store',
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
                trigger: 'axis', 
                axisPointer: {
                    type: 'cross'
                 },
            },
            legend: {
            data: ['Target Store'],
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