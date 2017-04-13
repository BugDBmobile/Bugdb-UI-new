<template>
<f7-page>
<f7-navbar title="Bug Info" back-link="Back" sliding>
</f7-navbar>

<p align="center">Bug Summary 1</p>
<f7-grid>
<f7-col><chart :options="chartData2" :style="{height:'375px', width:'375px'}"></chart></f7-col>
</f7-grid>

<p align="center">Bug Summary 2</p>
<f7-grid>
<f7-col><chart :options="chartData" :style="{height:'375px', width:'375px'}"></chart></f7-col>
</f7-grid>


<p align="center">Bug Summary 3</p>
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
                      data:['gavin','chris','guoyan','jinwei'],
                      align : "left",
                  },
                  xAxis : [
                      {
                          type : 'category',
                          data : ['Mon','Tu','We','th','Fri','Sar','sun']
                      }
                  ],
                  yAxis : [
                      {
                          type : 'value'
                      }
                  ],
                  series : [

                      {
                          name:'gavin',
                          type:'bar',
                          barWidth : 5,
                          stack: '搜索引擎',
                          data:[620, 732, 701, 734, 1090, 1130, 1120]
                      },
                      {
                          name:'chris',
                          type:'bar',
                          stack: '搜索引擎',
                          data:[120, 132, 101, 134, 290, 230, 220]
                      },
                      {
                          name:'guoyan',
                          type:'bar',
                          stack: '搜索引擎',
                          data:[60, 72, 71, 74, 190, 130, 110]
                      },
                      {
                          name:'jinwei',
                          type:'bar',
                          stack: '搜索引擎',
                          data:[62, 82, 91, 84, 109, 110, 120]
                      }
                  ]
            },
            chartData2: {
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
                          data:['gavin','chris','guoyan','jinwei'],
                          align : "left",
                      },
                      xAxis : [
                          {
                              type : 'category',
                              data : ['Mon','Tu','We','th','Fri','Sar','sun']
                          }
                      ],
                      yAxis : [
                          {
                              type : 'value'
                          }
                      ],
                      series : [

                          {
                              name:'gavin',
                              type:'line',
                              barWidth : 5,
                              stack: '搜索引擎',
                              data:[620, 732, 701, 734, 1090, 1130, 1120]
                          },
                          {
                              name:'chris',
                              type:'line',
                              stack: '搜索引擎',
                              data:[120, 132, 101, 134, 290, 230, 220]
                          },
                          {
                              name:'guoyan',
                              type:'line',
                              stack: '搜索引擎',
                              data:[60, 72, 71, 74, 190, 130, 110]
                          },
                          {
                              name:'jinwei',
                              type:'line',
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
                        data:['gavin','chris','guoyan','jinwei']
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
                                {value: idx * 128 + 80,  name:'gavin'},
                                {value: idx * 64  + 160,  name:'chris'},
                                {value: idx * 32  + 320,  name:'guoyan'},
                                {value: idx * 16  + 640,  name:'jinwei'}
                            ]
                        }
                    ]
            }
        }
     }
  }
</script>
