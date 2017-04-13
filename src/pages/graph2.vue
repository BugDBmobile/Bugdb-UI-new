<template>
<f7-page>
<p align="center">Bug Summary 1</p>
<f7-grid>
<f7-col><chart :options="chartData" :style="{height:'375px', width:'375px'}"></chart></f7-col>
</f7-grid>


<p align="center">Bug Summary 2</p>
<f7-grid>
<f7-col><chart :options="chartData1" :style="{height:'375px', width:'375px'}"></chart></f7-col>
</f7-grid>




</f7-page>
</template>

<style>

</style>

<script>
import ECharts from 'vue2-echarts/src/ECharts/ECharts.vue'

var idx = 1;
export default {
  components: {
     chart: ECharts
  },

  data: function () {
     return {
        chartData: {
                  tooltip : {
                      trigger: 'axis',
                      axisPointer : {            // 坐标轴指示器，坐标轴触发有效
                          type : 'shadow'        // 默认为直线，可选为：'line' | 'shadow'
                      }
                  },
                  title : {
                      show : false
                  },
                  legend: {
                      data:['百度','谷歌','必应','其他'],
                      align : "left",
                  },
                  xAxis : [
                      {
                          type : 'category',
                          data : ['周一','周二','周三','周四','周五','周六','周日']
                      }
                  ],
                  yAxis : [
                      {
                          type : 'value'
                      }
                  ],
                  series : [

                      {
                          name:'百度',
                          type:'bar',
                          barWidth : 5,
                          stack: '搜索引擎',
                          data:[620, 732, 701, 734, 1090, 1130, 1120]
                      },
                      {
                          name:'谷歌',
                          type:'bar',
                          stack: '搜索引擎',
                          data:[120, 132, 101, 134, 290, 230, 220]
                      },
                      {
                          name:'必应',
                          type:'bar',
                          stack: '搜索引擎',
                          data:[60, 72, 71, 74, 190, 130, 110]
                      },
                      {
                          name:'其他',
                          type:'bar',
                          stack: '搜索引擎',
                          data:[62, 82, 91, 84, 109, 110, 120]
                      }
                  ]
            },
            chartData1 : {
                    tooltip : {
                        trigger: 'item',
                        formatter: "{a} <br/>{b} : {c} ({d}%)"
                    },
                    legend: {
                        data:['Chrome','Firefox','Safari','IE9+','IE8-']
                    },
                    toolbox: {
                        show : true,
                    },
                    series : [
                        {
                            name:'bug Pie',
                            type:'pie',
                            center: ['50%', '45%'],
                            radius: '50%',
                            data:[
                                {value: idx * 128 + 80,  name:'Chrome'},
                                {value: idx * 64  + 160,  name:'Firefox'},
                                {value: idx * 32  + 320,  name:'Safari'},
                                {value: idx * 16  + 640,  name:'IE9+'},
                                {value: idx++ * 8  + 1280, name:'IE8-'}
                            ]
                        }
                    ]
            }
        }
     }
  }
</script>
