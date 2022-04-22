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
                symbolSize: 15,
                color: "#B71C1C",
                data: [
                    [997170, 25986],
                    [1050824, 7722],
                ]
                },
                {   
                    symbolSize: 20,
                    data: [
                            [1136295, 1351],
                            [1185260, 3536],
                            [1252088, 1151],
                            [1278391, 897],
                            [1285735, 625],
                            [1305197, 5425],
                            [1370193, 3597],
                            [1410272, 3413],
                            [1410518, 3492],
                            [1461693, 3343],
                            [1469152, 4259],
                            [1485331, [(2101 + 4745)/2]],
                            [1527335, [(2624 + 3288)/2]],
                            [1529826, [(555 + 3267)/2]],
                            [1548847, [(2800 + 2624)/2]],
                            [1755491, [(1321+4125)/2]],
                            [1994079, [(5720+3646)/2]],
                            [2036124, [(5293+4208)/2]]
                    ],
                    type: 'scatter',
                    name: 'Target Store',
                    color: '#CC0100'
                }
            ],
            tooltip: {
                trigger: 'axis',    
            },
            legend: {
            data: ['Target Store'],
            left: 'center',
            markPoint: {
        data: [
          { type: 'max', name: 'Max' },
          { type: 'min', name: 'Min' }
        ]
      },
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