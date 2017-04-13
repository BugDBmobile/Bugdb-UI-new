<template>
<f7-page>
<f7-navbar title="Bug Info" back-link="Back" sliding>
</f7-navbar>

<p align="center">Bug Summary 1</p>
<f7-grid>
<f7-col><chart :options="pieData" :style="{height:'375px', width:'375px'}"></chart></f7-col>
</f7-grid>

<!--
<p align="center">Bug Summary 2</p>
<f7-grid>
<f7-col><chart :options="lineData" :style="{height:'375px', width:'375px'}"></chart></f7-col>
</f7-grid>
-->
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
            pieData:{
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
                                {value: 3,  name:'gavin'},
                                {value: 5,  name:'chris'},
                                {value: 6,  name:'guoyan'},
                                {value: 4,  name:'jinwei'},
                            ]
                        }
                    ]
            },
            lineData:{
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
                              stack: 'bug num',
                              data:[5, 6, 7, 4, 3, 9, 10]
                          },
                          {
                              name:'chris',
                              type:'bar',
                              stack: 'bug num',
                              data:[10, 5, 4, 6, 2, 7, 9]
                          },
                          {
                              name:'guoyan',
                              type:'bar',
                              stack: 'bug num',
                              data:[12, 4, 7, 8, 9, 15, 4]
                          },
                          {
                              name:'jinwei',
                              type:'bar',
                              stack: 'bug num',
                              data:[13, 5, 9, 8, 7, 6, 4]
                          }
                      ]
                }
        }
  },
  mounted(){
      let query = this.$route.query;
       let  userId = query['userId'];
       let startTime= query['startTime'];
       let isclose = query['isclose'];
       let url1 = "statistic?"+"userId="+userId+"&startTime="+startTime+"&isClose="+isclose+"&endTime=";
      this.$http({url: url1, method: 'GET'}).then((response) =>
      {
          let data = response.data;
          console.log(data);
          var  name = new Array();
          var  value = new Array();
          var  sdata = new Array();
          for(var key in data) {
               value.push(data[key]);
              if(key == 1) name.push('chris');
              if(key == 2) name.push('guoyan');
              if(key ==3 ) name.push('gavin');
              if(key ==4 ) name.push('jinwei');
         }
         console.log(name);
         console.log(value);
         for(var i =0; i < name.length; i++){
              sdata.push({'name':name[i],'value':value[i]+5});
         }
         console.log(sdata);
          var pieOptions= {
                  tooltip : {
                      trigger: 'item',
                      formatter: "{a} <br/>{b} : {c} ({d}%)"
                  },
                  legend: {
                      data: name
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
                          data: sdata
                      }
                  ]
          };
          console.log(pieOptions);
          this.pieData = pieOptions;

      },(response) => {
          console.log("data null");
      });

      let url2="statistic/day?userId="+userId+"&startTime="+startTime+"&isClose="+isclose+"&endTime=";
      this.$http({url: url2, method: 'GET'}).then((response) =>
      {

          var lineOptions= {
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
                        data: name,
                        align : "left",
                    },
                    xAxis : [
                        {
                            type : 'category',
                            data : data
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
                            data:[620, 732, 701, 734, 1090, 1130, 1120]
                        },
                        {
                            name:'chris',
                            type:'line',
                            data:[120, 132, 101, 134, 290, 230, 220]
                        },
                        {
                            name:'guoyan',
                            type:'line',
                            data:[60, 72, 71, 74, 190, 130, 110]
                        },
                        {
                            name:'jinwei',
                            type:'bar',
                            data:[62, 82, 91, 84, 109, 110, 120]
                        }
                    ]
              };
          //  this.pieData=lineOptions;
      },(response) => {
          console.log("data null");
      });
  }
}
</script>
